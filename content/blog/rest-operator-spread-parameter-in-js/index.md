---
title: The rest parameter and spread operator in Javascript
date: "2020-01-07"
description: "What is the ... prefix in Javascript used for? Simple explanations of ES6 extended parameter handling backed with code examples."
---

## Introduction

ECMAScript 2015 (also known as ES6) is the sixth edition of the ECMAScript Language Specification.

It introduced a several interesting and useful additions to the Standard library and consequently the Javascript language implementation across diff. browsers, like the **let and const keywords, arrow functions, classes, default parameter values** and the **rest and spread operators**.

This article focuses on the _rest and spread operators_ which both have the same syntax (the ... prefix followed by a parameter identifier), but different functionality, based in which context they're used.

## Rest Parameter

**The purpose of the rest parameter is to aggregate or collapse the remaining arguments of a function, after all the named arguments, into a single array.**

So only the last parameter is the **rest parameter**, and it contains all of the remaining arguments as members of the same array.

If you have a function definition which looks like this:

```javascript
function example(a, b, ...c) {}
```

and you call the function with the following arguments:

```javascript
example("hey", "there", true, 50, 100)
```

Then the following assertions equal true;

```javascript
assert a.equals("hey"); // true
assert b.equals("there"); // true
assert c.deepEquals([true, 50, 100]) // true
```

All of the _example_ function parameters, after the first two named arguments (**a** and **b**), are aggregated into a single array (under the _rest_ parameter **c**).

For example, look at the following code snippet and the implementation of the _sumAllArgs_ function:

```javascript
function sumArrayItems(arr) {
  const arrLen = arr.length
  let sum = 0

  for (let i = 0; i < arrLen; i++) {
    sum += arr[i]
  }

  return sum
}

function sumAllArgs(a, b, c ...restOfArgs) {
  return a + b + sumArrayItems(restOfArgs)
}

const result = sumAllArgs(1, 2, 3, 4, 5, 6, 7);
console.log(result); // 28
```

The _sumAllArgs_ functions returns the sum of all arguments passed to it. Since there are more than 3 named arguments (a, b, c), the rest are aggregated into an array under the _rest_ parameter identifier **restOfArgs**.

Then the _sumArrayItems_ function is called which, as the name suggests, sums all the array items and returns the result back to the _sumAllArgs_ function.

### Pre-ES6 "hacks" using the arguments object and the Array.prototype.slice() method

Inside all non-arrow functions a local variable called **arguments** is available. It is an object which refers to the functions arguments. If the function is passed n arguments, they can be accessed like this:

```javascript
function example() {
  const arg0 = arguments[0]
  const arg1 = arguments[1]
  // ....
  const arg8 = arguments[8]

  console.log(arg0) // 1
  console.log(arg1) // 2
  // ...
  console.log(arg8) // 9
}

example(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

The arguments object is not an Array, but it is array-like in the sense that it has a **length** property. But this is pretty much where the similarity ends. The arguments object doesn't have any of the Array methods like push() or pop().

But, it can be converted into a real array by using a hack like this:

```javascript
const args = Array.prototype.slice.call(arguments)
```

The slice() method is being accessed directly from the Array.prototype object (for efficiency reasons). It could also be accessed directly from an array instance like this:

```javascript
const args = [].slice.call(arguments)
```

So before ES6, and the emergence of the rest parameter, this is how the _sumAllArgs_ would be implemented:

```javascript
function sumAllArgs(a, b) {
  const restOfArgs = Array.prototype.slice.call(arguments, 2)
  return a + b + sumArrayItems(restOfArgs)
}
```

The slice method has a second argument (2), which is used to skip over the first two named arguments (a and b).

## Spread Operator

**The spread syntax "spreads" out an iterable (like an array, string or set) inside a reciever.**

Iterables are a generalization of arrays. It is a concept that allows to make any object useable in a for..of loop. To make any range of values iterable a Symbol.iterator (built-in) object is used.

I won't go into more detail regarding the topic of Iterables because I don't want to digress from the current topic. If you wan't to know more about this concept [check out this link](https://javascript.info/iterable).

Here's an example of the spread operator used on a string (**iterable**) inside an array (**receiver**).

```javascript
const name = "Mike"
const letters = [...name]

console.log(letters) // ["M", "i", "k", "e"]
```

The three dots act on the string "Mike" and "spread out" each individual character inside the receiver (the **letters** array).

Arrays can also be "spread" into other arrays. In fact, this is how merging and copying of javascript objects (which includes arrays) is done these days.

Here is an example of array **shallow** copying:

```javascript
const cars = ["Audi", "Mercedes", "BMW"]
const copiedCars = [...cars]

cars[0] = "null"

console.log(cars) // ["null", "Mercedes", "BMW"]
console.log(copiedCars) // ["Audi", "Mercedes", "BMW"]
```

For deep copying of arrays or objects, try the following:

```javascript
const arr1 = JSON.parse(JSON.stringify(arr2))

// or you can use a third party library like lodash
```

Here is an example of **shallow** merging of two arrays:

```javascript
const cars1 = ["Audi", "Mercedes", "BMW"]
const cars2 = ["Ford", "Hyundai"]

const cars2 = [...cars1]

console.log(cars1) // ["Audi", "Mercedes", "BMW"]
console.log(cars2) // ["Audi", "Mercedes", "BMW", "Ford", "Hyundai"]
```

For more info about deep vs shallow copying in JS [check out this link](https://www.freecodecamp.org/news/copying-stuff-in-javascript-how-to-differentiate-between-deep-and-shallow-copies-b6d8c1ef09cd/)

It is also possible to shallow merge and copy objects. Since every object in JS is an Iterable, we can pluck the individual property-value pairs from n different objects, and store them inside the appropriate receiver.

Example:

```javascript
const person1 = {
  name: "Mike",
  age: 28,
}

const person2 = {
  name: "Jane",
  age: 25,
}

const person3 = {
  hairColor: "black",
  glasses: true,
}

const mix = {
  ...person1,
  ...person2,
  ...person3,
}

console.log(mix)

/* 
 {
    name: "Jane",
    age: 25,
    hairColor: "black",
    glasses: true
 }
*/
```

It is visible in the previous example that it is possible to merge multiple objects together.

Properties that have the same name (**name** and **age** from objects _person1_ and _person2_) are overwritten by the values from the last object; indicated by the spread operator furthest to the right of the expression (_person2_ overwrites _person1_).

The properties **hairColor** and **glasses**, from _person3_, are unique so they are simply copied into the _mix_ object.

## Conclusion

For more info, check out [ECMAScript 6 - ECMAScript 2015 Overview](http://es6-features.org/#Constants)

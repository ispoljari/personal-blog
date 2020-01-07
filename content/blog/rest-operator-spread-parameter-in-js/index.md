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

The purpose of the rest parameter is to aggregate or collapse the remaining arguments of a function, after all the named arguments, into a single array.

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

Before ES6, and the emergence of the rest parameter, this is how the _sumAllArgs_ was implemented:

```javascript
function sumAllArgs(a, b) {
  const restOfArgs = Array.prototype.slice.call(arguments, 2)
  return a + b + sumArrayItems(restOfArgs)
}
```

## Spread Operator

## Conclusion

For more info, check out [ECMAScript 6 - ECMAScript 2015 Overview](http://es6-features.org/#Constants)

# Functions
Create a `Function` object that have access to methods, properties etc.

## Table of Contents
- [Function Declaration vs. Function Expression](#function-declaration-vs-function-expression)
- [Arrow Functions](#arrow-functions)
- [Immediately Invoked Function Expressions (IIFE)](#immediately-invoked-function-expressions-iife)
- [Higher Order Functions](#higher-order-functions)

## Function Declaration vs. Function Expression
### Function Declaration
- Syntax:
  ```js
  function name([param[, param,[..., param]]]) {
    // statements
  }
  ```
- Example: `function doStuff() {};`
- **Function declarations are hoisted to the top of the enclosing block or global scope, hence can be used before it has been declared.**

### Function Expression
- Syntax:
  ```js
  const variable = function [name]([param1[, param2[, ..., paramN]]]) {
     // statements
  }
  ```
- Example: `const doStuff = function() {};`
- **The reference to a function expression is stored in a variable, which can now be used to call the function.**
  - Meaning that `doStuff` is not the name of the function, and is merely a reference to the function.
  - Even if the function is given a name (Ex: `const doStuff = function doingStuff() {}`), the variable name has to be used to call the function.
- **Function expressions are not hoisted.**
- **The function name can be omitted to create *anonymous* (unnamed) functions.**
- A function expression can be used as an IIFE, which runs as soon as it is defined.
#### Named Function Expressions
- When a function expression is assigned to a variable, it has a name property.
- Use named function expressions if you want to refer to the current function inside the function body.
- This name is local only to the function scope.
```js
// If function name is omitted, the variable name is the name.
let foo() = function() {};
foo.name // "foo"

// The name stays the same despite reassigning a different variable.
let foo2() = foo;
foo2.name = "foo";

// If function name is present, the function name is the name.
let bar = function baz() {};
bar.name // "baz"
```
### When to Use Each?
- Choosing one over the other requires thinking about when and where the function is needed (one time use only? or used again).
- *Function expressions are invoked to **avoid polluting the global scope.***
  - Instead of your program being aware of many different functions, use anonymous functions as they are used and forgotten immediately.

## Arrow Functions
- Especially useful for when the function only exists to be passed in to another function.
- Syntax: `const funcName = () => {}`
### Implicit Returns
- Only work with arrow functions that have only one expression in the body.
- No need for return keyword and replace `{}` with `()`.
  - *The `()` indicates that only one thing will be returned.*
### Arrow Functions and `this` Keyword
- The keyword `this` when used inside of an arrow function will have the same value as the keyword `this` in the scope of where the function was created.
  - i.e., arrow functions perform **lexical binding** and uses the surrounding scope as the scope of `this`.

## Immediately Invoked Function Expressions (IIFE)
- A function is created at the same time it is called.
- Example: `(function() => {}))` or `(() => {})()`

## Higher Order Functions
- **A function that takes a function as an argument OR returns a function.**
### Taking a Function as an Argument
- JavaScript has built-in higher order functions that take functions as arguments.
- For example, `Array.prototype.map()`, `Array.prototype.filter()` calls the received function on each element and returns a new array.
#### Example
```js
let nums = [1, 2, 3];

const double = (arr) => {
  const doubled = [];
  for (let el of arr) {
    doubled.push(el * 2);
  }
  return doubled;
}

console.log(double(nums)); // [2, 4, 6]
console.log(nums); // [1, 2, 3]

// OR

nums.map(el => el * 2);
```
### Returning a Function
- Higher Order Functions can serve as "function factories" that "distribute" functions.
- Useful for when you want to start with some base functionality and then extend it with some dynamic data.
#### Examples
##### Example 1
```js
const calculate = (operation) => {
  switch (operation) {
    case "ADD":
      return function (a, b) {
        return a + b;
      };
    case "SUBTRACT":
      return function (a, b) {
        return a - b;
      };
    case "MULTIPLY":
      return function (a, b) {
        return a * b;
      }
    case "DIVIDE":
      return function (a, b) {
        return a / b;
      }
  }
}

const addition = calculate("ADD");
addition(3, 5); // 8

// OR
calculate("ADD")(3, 5); // 8
```
- Invoking `calculate("ADD")` returns an anonymous function under the case `"ADD"`, which we store in `addition`.
- Now, we can invoke the return function by calling `addition()` with the required arguments. 
##### Example 2
- Assume we need to append strings with emojis.
```js
// This base function can be used to compose more complex functions.
const appendEmoji = (fixed) => {
  return (dynamic) => fixed + dynamic; // The inner function takes both arguments and adds them together.
}

// Use the base function to create more specialized functions that point to a specific emoji.
const rain = appendEmoji("🌧");
const sun = appendEmoji("🌞");

console.log(rain(" today"); // 🌧 today
console.log(sun(" tomorrow"); // 🌞 tomorrow
```
- This code does not rely on any shared state.

## Reference
[Function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)  
[JavaScript - 함수 선언문, 함수 표현식 그리고 화살표 함수 비교](https://velog.io/@bigbrothershin/%EC%98%A4%EB%8A%98-%EA%B3%B5%EB%B6%80%ED%95%A0-%EA%B2%832020.1.30)  
[Higher Order Functions in JavaScript – Beginner's Guide](https://www.freecodecamp.org/news/higher-order-functions-in-javascript/)  
[Object Oriented vs Functional Programming with TypeScript - YouTube](https://www.youtube.com/watch?v=fsVL_xrYO0w&ab_channel=Fireship)  

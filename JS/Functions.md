# Functions

## Table of Contents
- [What is a Function?](#what-is-a-function)
- [Function Declaration vs. Function Expression](#function-declaration-vs-function-expression)
- [Arrow Functions](#arrow-functions)
- [Immediately Invoked Function Expressions (IIFE)](#immediately-invoked-function-expressions-iife)

## What is a Function?
> Generally speaking, a function is a "subprogram" that can be _called_ by code external (or internal, in the case of recursion) to the function. Like the program itself, a function is composed of a sequence of statements called the function body. Values can be passed to a function as parameters, and the function will return a value. | MDN

- A function is a **first-class citizen**.
  - This means that a function can be assigned to variables and properties, passed on as an argument to another function, and can be returned from another function.
    - i.e. functions can also be handled like an object.
  - This allows for abstractions and code reuse.
- Separation of Concerns
  - In FP, the unit of separation is functions.
  - cf. in OOP, the unit of separation is objects.
### What is an Adequate Length of a Function?
> The argument that makes most sense to me, however, is the **separation between intention and implementation**. If you have to spend effort into looking at a fragment of code to figure out what it's doing, then you should extract it into a function and name the function after that “what”. That way when you read it again, the purpose of the function leaps right out at you, and most of the time you won't need to care about how the function fulfills its purpose - which is the body of the function. | Martin Fowler

- Everyone has different opinions on this matter for various reasons.
- A function that is too large should most likely be broken down into smaller functions.
- A function that is too small may indicate that it was unnecessary to break it down.

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

## Reference
[Functions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)  
[design - What is the ideal length of a method for you? - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/133404/what-is-the-ideal-length-of-a-method-for-you)  
[FunctionLength - Martin Fowler](https://martinfowler.com/bliki/FunctionLength.html)  
[Function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)  
[JavaScript - 함수 선언문, 함수 표현식 그리고 화살표 함수 비교](https://velog.io/@bigbrothershin/%EC%98%A4%EB%8A%98-%EA%B3%B5%EB%B6%80%ED%95%A0-%EA%B2%832020.1.30)  

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

## Immediately Invoked Function Expressions (IIFE)
- A function is created at the same time it is called.
- Example: `(function() => {}))` or `(() => {})()`

## Higher Order Functions
- Functions that operate on/with other functions. They can:
  - accept other functions as arguments.
    - Example
      ```js
      function callTwice(func) {
        func();
        func();
      }
      
      function laugh() {
        console.log("hahaha";
      }
      
      callTwice(laugh);
      ```
  - return a function.
    - "Function factories"
      - The higher order function is making a new function.
    - Example
      ```js
      function makeBetweenFunc(min, max) {
        return function (val) {
          return val >= min && val <= max;
        }
      }
      
      const inAgeRange = makeBetweenFunc(18, 100);
      inAgeRange(17); // false
      inAgeRange(68); // true
      
      const isChild = makeBetweenFunc(0, 17);
      isChild(5); // true
      isChild(35); // false
      
      const isInNineties = makeBetweenFunc(1990, 1999);
      isInNineties(1996); // true
      isInNineties(2000); // false
      ```

## Reference
[Function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)  
[JavaScript - 함수 선언문, 함수 표현식 그리고 화살표 함수 비교](https://velog.io/@bigbrothershin/%EC%98%A4%EB%8A%98-%EA%B3%B5%EB%B6%80%ED%95%A0-%EA%B2%832020.1.30)  

# Functions
Create a `Function` object that have access to methods, properties etc.

## Function Declaration vs. Function Expression
### Function Declaration
- Syntax:
```
function name([param[, param,[..., param]]]) {
  statements
}
```
- Example: `function doStuff() {};`
- **Function declarations are hoisted to the top of the enclosing block or global scope, hence can be used before it has been declared.**

### Function Expression
- Syntax:
```
const variable = function [name]([param1[, param2[, ..., paramN]]]) {
   statements
}
```
- Example: `const doStuff = function() {};`
- **The reference to a function expression is stored in a variable, which can now be used to call the function.**
  - Meaning that `doStuff` is not the name of the function, and is merely a reference to the function.
  - Even if the function is given a name (Ex: `const doStuff = function doingStuff() {}`), the variable name has to be used to call the function.
- Function expressions are not hoisted.
- The function name can be omitted to create *anonymous* (unnamed) functions.
- Immediately Invoked Function Expressions run as soon as they are defined.
  - Functions that are used only once.
#### Named Function Expressions
- When a function expression is assigned to a variable, it has a name property.
```
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

## Reference
[Function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)  
[JavaScript - 함수 선언문, 함수 표현식 그리고 화살표 함수 비교](https://velog.io/@bigbrothershin/%EC%98%A4%EB%8A%98-%EA%B3%B5%EB%B6%80%ED%95%A0-%EA%B2%832020.1.30)

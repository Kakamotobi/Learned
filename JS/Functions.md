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
- Function declarations are hoisted to the top of the enclosing block or global scope, hence can be used before it has been declared.

### Function Expression
- Syntax:
```
const variable = function [name]([param1[, param2[, ..., paramN]]]) {
   statements
}
```
- Example: `const doStuff = function() {};`
- A function expression is stored in a variable, which can now be used as a function.
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

## Reference
[Function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)

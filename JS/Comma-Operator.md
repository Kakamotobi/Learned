# Comma Operator (`,`)
> The **comma operator (`,`)** evaluates each of its operands (from left to right) and returns the value of the last operand. This lets you create a compound expression in which multiple expressions are evaluated, with the compound expression's final value being the value of the rightmost of its member expressions. | MDN

- Syntax: `expr1, expr2, expr3`.
- Useful for when including multiple expressions where only a single expression is required.
  - Usually wrapped in `()`.

## Examples
```js
let a, b, c;
```
```js
const a = (console.log("hello"), 7);
// "hello"
a; // 7
```
```js
let x = 0;
let y = 0;
let z = (x++, y--);
console.log(x, y, z); // 1, -1, 0
```
```js
++x;
// same as
(x += 1, x);
```
```js
for (let i = 0, j = 10; i <= 10; i++, j--) {
  // do something.
}
```

## Reference
[Comma operator (,) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator)  

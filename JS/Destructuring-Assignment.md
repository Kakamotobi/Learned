# Destructuring Assignment
> Javascript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

## Destructuring Arrays
### Syntax
```
const [a, b, ...rest] = [10, 20, 30, 40, 50]
// OR
let a, b, rest;
[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]
```
## Destructuring Objects
### Syntax
```
const {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
// OR
let a, b, rest;
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});

console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```

## Reference
[Destructuring assignment - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

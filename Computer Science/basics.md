# Basics

## Table of Contents
- [Typing](#typing)

## Typing
### Static Typing vs. Dynamic Typing
#### Static Typing
- Variable types are checked at "compile-time" or before runtime.
- So, compilation error will be thrown before the code runs.
- Ex: if a function receives a string as an argument, when in fact it's expecting a number, a compilation error is thrown.
#### Dynamic Typing
- Variable types are checked on the go as the code is executed.
- Ex: a function accepts any type of argument, and only knows which type it is when the code is executed.
### Strong Typing vs. Weak Typing
### Strong Typing
- Converts a variable or value's type to suit the current situation automatically.
- Ex: "123" will always be treated as a string (never used as a number unless otherwise manually converted).
- Python Example
  ```py
  x = 1
  y = "2"
  z = x + y # TypeError
  ```
### Weak Typing
- The interpreter or compiler attempts to make the best of what it is given.
- Ex: in some situations an integer miht be treated as if it were a string to suit the context of the situation.
- JavaScript Example
  ```js
  const x = 1;
  const y = "2";
  const z = x + y; // "12" because JS treated both values as strings and concatenated them together.
  ```
### Reference
[Understanding Types: Static vs. Dynamic, & Strong vs. Weak](https://medium.com/@cpave3/understanding-types-static-vs-dynamic-strong-vs-weak-88a4e1f0ed5f)

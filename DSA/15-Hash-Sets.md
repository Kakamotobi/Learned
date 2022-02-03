# Hash Sets

## Table of Content
- [What are Hash Sets?](#what-are-hash-sets)
- [Sets in JavaScript](#hash-sets-in-javascript)

## What are Hash Sets?
- A hash set is an unordered collection of items in which duplicate values cannot be stored.
  - Hash sets are unordered because each value's distinguished hash is used to determine their order.
- *The underlying data structure is a hash table.*
### How it Works
- With a hash table, the key gets hashed. With a hash set, there is no key. Therefore, the value is hashed.
  - The result of the hash will be used to determine at what position to store it in memory.
- The reason there cannot be a duplicate is because hashing the same value will result to the same hash/position.
### Hash Sets Big O
- Insertion - O(1)
- Removal - O(1)
- `Set.prototype.has(val)` - O(1)
  - *O(1) time complexity is not guaranteed. However, the ECMAScript specification requires the method to run in sublinear time.*

## Hash Sets in JavaScript
### `Set` (ES6)
- `Set` objects are collections of values.
- `Set` allows storage of unique values of any data type (primitive values, object references, etc).
- `Set` is unordered but always has the same iteration order (insertion order).

## Reference
[Set - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)  
[ECMAScript 2022 Language Specification](https://tc39.es/ecma262/multipage/keyed-collections.html#sec-set-objects)  
[Stack Overflow Thread](https://stackoverflow.com/questions/55057200/is-the-set-has-method-o1-and-array-indexof-on)

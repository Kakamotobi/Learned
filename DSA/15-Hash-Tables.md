# Hash Tables/Maps

## Table of Content
- [What are Hash Tables/Maps?](#what-are-hash-tablesmaps)
  - [Hash Table Big O](#hash-table-big-o)
- [Hash Tables in JavaScript](#hash-tables-in-javascript)
  - [Objects](#objects)
  - [`Map` (ES6)](#map-es6)
- [Hash Functions](#hash-functions)
  - [What Makes a Good Hash Function?](#what-makes-a-good-hash-function)
  - [Hash Function Example for Strings](#hash-function-example-for-strings)
  - [Improved Version](#improved-version)
  - [Handling Collisions](#handling-collisions)
- [Hash Table Demo Implementation](#hash-table-demo-implementation)

## What are Hash Tables/Maps?
- A hash table is a data structure used to store key-value pairs.
- Unlike arrays:
  - Hash tables are not ordered.
  - Hash tables are fast at not only finding, but also at adding and removing values.
- Built-in to most programming languages.
  - JS: objects and Maps.
  - Python: dictionaries.
  - Java, Go, Scala: Maps.
  - Ruby: Hashes.
### Hash Table Big O
- Insertion - O(1)
- Removal - O(1)
- Access - O(1)
- *Comes down to how good the hash function is.*

## Hash Tables in JavaScript
### Objects
- Objects only allow strings, or symbols as keys.
  - *Numbers can be used but will be converted to strings.*
- Objects allow any data types as values.
- Objects are ***non-iterable (non-ordered).***
  - i.e. the sequence of insertion of key-value pairs is not remembered.
### `Map` (ES6)
- A unique key maps to a value.
- A `Map` allows any data types for both keys and values.
- A `Map` is an ***iterable data structure (ordered).***
  - i.e. the sequence of insertion of key-value pairs is remembered.
  - Hence, `for...of` loops can be used.
- A `Map` inherits properties and methods from `Map.prototype`.

## Hash Functions
> A hash function is a function that can be used to map data of arbitrary size to fixed-size values.
- In order to look up values by key, we need a way to convert keys into valid array indices. A hash function does just this.
  - Ex: for this key-value pair `["pink", "#ff69b4"]`, `"pink"` should be converted to a valid array index (ex: `0`).
### What Makes a Good Hash Function?
- Fast (i.e. constant time).
- Doesn't cluster outputs at specific indices, but distributes uniformly.
  - Prime number in the hash helps in spreading out the keys more uniformly.
  - Also helps if the array that you're putting values into has a prime length.
- Deterministic (the same input always yields the same output).
### Hash Function Example for Strings
```js
// arrLen is used to get an index between 0 and arrLen-1.
const hash = (key, arrLen) => {
  let hashVal = 0;
  for (let char of key) {
    // map "a" to 1, "b" to 2, "c" to 3, etc.
    let value = char.charCodeAt(0) - 96;
    hashVal = (hashVal + value) % arrLen;
  }
  return hashVal;
}
```
```js
hash("pink", 10); // 0
hash("orangered", 10); // 7
hash("cyan", 10); // 3
```
#### Issues with this Hash Function
1. Only hashes strings.
2. Not constant time - linear in key length.
3. Could be a little more random.
### Improved Version
```js
const hash = (key, arrLen) => {
  let hashVal = 0;
  // Use prime number to help distribute uniformly.
  let WEIRD_PRIME = 31;
  // Set maximum loops to 100.
  for (let i = 0; i < Math.min(key.length, 100); i++) {
    let char = key[i];
    let value = char.charCodeAt(0) - 96;
    hashVal = (hashVal * WEIRD_PRIME + value) % arrLen;
  }
  return hashVal;
}
```
```js
hash("hello", 13) // 7
hash("goodbye", 13) // 9
hash("hi", 13) // 10
```
### Handling Collisions
- Even with a large array and a great hash function, collisions are inevitable.
#### Method 1 - Separate Chaining
- Store the items at the same index using another nested data structure (ex: array, linked list).
- Allows us to store multiple key-value pairs at the same index.
  - Hence, we can store more data than the length of our array.
- For example, if `"pink"` and `"cyan"` share the same hash/index, to get `"pink"`, we go to that index, then loop through until we find `"pink"`.
#### Method 2 - Linear Probing
- If there is a collision, search through the array to find the next empty slot.
- Allows us to store a single key-value pair at each index.
  - We can only store data until the length of our array.
- For example, if `"cyan"` is already placed and `"pink"` results to the same hash/index as `"cyan"`, find the next empty index from there on.

## Hash Table Demo Implementation
```js
class HashTable {
  constructor(size = 53) {
    this.keyMap = new Array(size);
  }
  
  _hash(key) {
    let hashVal = 0;
    let WEIRD_PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      let char = key[i];
      let value = char.charCodeAt(0) - 96;
      hashVal = (hashVal * WEIRD_PRIME + value) % this.keyMap.length;
    }
    return hashVal;
  }
  
  // Store a key-value pair in the hash table.
  // When key already exists, most languages overwrite it with the new val.
  set(key, val) {
    // Hash the key (figure out where to store it).
    const hashVal = this._hash(key);
    // Store the key-value pair via separate chaining.
    if (!this.keyMap[hashVal]) {
      this.keyMap[hashVal] = [];
    }
    this.keyMap[hashVal].push([key, val]);
  }
  
  // Access the value of a particular key.
  get(key) {
    // Hash the key
    const hashVal = this._hash(key);
    // Retrieve the key-value pair in the hash table.
    if (this.keyMap[hashVal]) {
      for (let i = 0; i < this.keyMap[hashVal].length; i++) {
        if (this.keyMap[hashVal][i][0] === key) {
          return this.keyMap[hashVal][i][1];
        }
      }
    }
    // If the key is not found, return undefined.
    return undefined;
  }
  
  // Return an array of the keys in the table.
  keys() {
    const keysArr = [];
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          if (!keysArr.includes(this.keyMap[i][j][0])) {
            keysArr.push(this.keyMap[i][j][0];
          }
        }
      }
    }
    return keysArr;
  }
  
  // Return an array of the (unique) values in the table.
  values() {
    const valuesArr = [];
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          if (!valuesArr.includes(this.keyMap[i][j][1])) {
            valuesArr.push(this.keyMap[i][j][1];
          }
        }
      }
    }
    return valuesArr;
  }
  
  // Return an array of the key-value pairs in the table.
  entries() {
    const entriesArr = [];
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          entriesArr.push(this.keyMap[i][j];
        }
      }
    }
    return entriesArr;
  }
}
```

## Reference
[Hash function - Wikipedia](https://en.wikipedia.org/wiki/Hash_function)  

# Objects

## Table of Contents
- [Disclaimer](#disclaimer)
  - [The 7 Main JavaScript Language Types](#the-7-main-javascript-language-types)
  - [Creating Objects](#creating-objects)
  - [JavaScript Language Type/Sub-type Classifications](#javascript-language-typesub-type-classifications)
  - [The Disclaimer Explained](#the-disclaimer-explained)
- [Working with Object Literals](#working-with-object-literals)
  - [Shorthand Object Properties](#shorthand-object-properties)
  - [Computed Property Names](#computed-property-names)
  - [Shorthand for Adding Methods to Objects](#shorthand-for-adding-methods-to-objects)

## Disclaimer
- **"Everything in JavaScript is an object" is untrue.**
- The term "object" is used a lot in the world of JavaScript and it is causing a lot of confusion and misunderstandings.
### The 7 Main JavaScript Language Types
- boolean
- number
- string
- object
- symbol
- null
- undefined
### Creating Objects
- There are two ways to create objects: **1) the declarative way (literals)**, and **2) imperative way (constructor/built-in objects)**.
  - Literals:
    - boolean
    - number
    - string
    - object (object literal, array literal)
    - symbol
    - *null and undefined do not have literals as they are just `null` and `undefined` in and of itself.*
  - Built-in Objects (objects in the global scope):
    - Boolean
    - Number
    - String
    - Object
    - Array
    - Symbol
    - Function
    - Date
    - RegExp
    - Error
    - *These look like "real" classes (such as in Java) but they are simply functions that are used as constructors to generate an object of their sub-type.*
### JavaScript Language Type/Sub-type Classifications
- Primitives - data that is not an object and has no methods or properties.
  - Simple Primitives
    - boolean, string, number, null, undefined.
    - *Despite `typeof null` returning an `'object'`, it is not an object.*
  - Objects
    - (Ordinary) Objects
      - Object literals, array literals.
    - Built-in Objects
    - Callable Objects
      - Functions
        - Functions are 'first class' as they can be passed as arguments to other functions, return another function, be referenced through a variable, and/or be stored in a data structure.
### The Disclaimer Explained
- The only reason that primitives behave like objects (Ex: `"abc".length` or `"abc".toUpperCase()`) is because **JavaScript automatically coerces literals to their respective built-in objects**.
  - `"abc".length` implicitly creates a `String` wrapper object and calls the `String.prototype.length` on that object - this is JavaScript's infamous Prototypal Inheritance.
  - This is also why we use literals (declarative) more than constructors (imperative).
    ```js
    // Literal
    const strLiteral = "hello";
    typeof strLiteral; // 'string'
    console.log(strLiteral); // 'hello'
    Object.prototype.toString.call(strLiteral); // '[object String]'
    
    // Constructor
    const strConstructor = new String("hello");
    typeof strConstructor; // 'object'
    console.log(strConstructor); // String {'hello'}
    Object.prototype.toString.call(strLiteral); // '[object String]'
    ```
      - We can see that both `strLiteral`'s and `strConstructor`'s constructor/sub-type is the built-in object `String`.
        - `Object.prototype.toString.call(num)` will result to `[object Number]`.


## Working with Object Literals
### Shorthand Object Properties
- Including a variable in an object literal will automatically assign its value as well.
#### Example
```js
// Previously
const getStates = (arr) => {
  const max = Math.max(...arr);
  const min = Math.min(...arr);
  const sum = arr.reduce((sum, r) => sum + r);
  const avg = sum / arr.length;
  return { // Returning an object literal containing the variables and their values as key-value pairs.
    max: max,
    min: min,
    sum: sum,
    avg: avg,
  }
}

// Shorthand
const getStates = (arr) => {
  const max = Math.max(...arr);
  const min = Math.min(...arr);
  const sum = arr.reduce((sum, r) => sum + r);
  const avg = sum / arr.length;
  return { // Returning an object literal containing the variables and their values as key-value pairs.
    max,
    min,
    sum,
    avg
  }
}
```
### Computed Property Names
- Improvement for object literal syntax.
- Allows the names of object properties in JS object literal notation to be determined dynamically, i.e., computed.
- We can use a variable as a key name in an object literal property.
- Use when: 
  - creating functions that return objects.
  - adding things (ex: dynamic key) into objects.
#### Dynamic Key
- The value of the variable that is in `[]` will become the name of the key.
```js
let appSate = "loading";
const app = {
  [appState] = true;
}
app; { loading: true }
```
#### Example 1
```js
const role = "host";
const person = "Tom Jerry";

// Previously (2 step process)
const cartoon = {};
cartoon[role] = person;
cartoon; // { host: "Tom Jerry" }

// Computed Properties
const cartoon = {
  [role]: person
}
cartoon; // { host: "Tom Jerry" }
```
#### Example 2	
```js
const state = {
  loading: true,
  name: "",
  job: ""
}

// whatever key that is passed, will be the one that will be updated.
function updateState(obj, key, value) {
  obj[key] = value;
}

updateState(state, "name", "john");
state; // { loading: true, name: "john", job: "" }
```

## Shorthand for Adding Methods to Objects
### Example
```js
const math = {
  blah: "Hi!",
  add(x, y) {
    return x + y;
  },
  multiply(x, y) {
    return x * y;
  }
}

math.add(2, 3); // 5
```

## Reference
You Don't Know JS - Kyle Simpson  
[Dynamic Object Keys](https://www.youtube.com/watch?v=_qxCYtWm0tw)  

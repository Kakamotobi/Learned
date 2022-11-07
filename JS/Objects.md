# Objects

## Table of Contents
- [Disclaimer](#disclaimer)
  - [The 7 Main JavaScript Language Types](#the-7-main-javascript-language-types)
  - [Creating Objects](#creating-objects)
  - [JavaScript Language Type/Sub-type Classifications](#javascript-language-typesub-type-classifications)
  - [The Disclaimer Explained](#the-disclaimer-explained)
- [Anatomy of Objects](#anatomy-of-objects)
  - [Properties vs Methods](#properties-vs-methods)
  - [Arrays](#arrays)
  - [Objects](#objects)
    - [Property Descriptor](#property-descriptor)
    - [Accessing Object Properties](#accessing-object-properties)
      - [`[[Get]]`](#get)
      - [`[[Put]]`](#put)
      - [Getters and Setters](#getters-and-setters)
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
      - Objects, arrays, functions and regular expressions are all objects, irrespective of their form (literal/constructor).
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
    strLiteral.length; // 5
    typeof strLiteral; // 'string'
    console.log(strLiteral); // 'hello'
    Object.prototype.toString.call(strLiteral); // '[object String]'
    
    // Constructor
    const strConstructor = new String("hello");
    strConstructor.length; // 5
    typeof strConstructor; // 'object'
    console.log(strConstructor); // String {'hello'}
    Object.prototype.toString.call(strLiteral); // '[object String]'
    ```
      - We can see that both `strLiteral`'s and `strConstructor`'s constructor/sub-type is the built-in object `String`.
        - `Object.prototype.toString.call(num)` will result to `[object Number]`.

## Anatomy of Objects
- An **Object Container** (i.e. an object) contains the pointer (reference) to where the property's value is placed at (NOT the actual value itself).
  - Property names (keys) of objects are always converted to strings.
### Properties vs Methods
- A function in an object (whether it was declared in that object or not) does not mean that it is a method tied to that object. Rather, it is just a reference to that function.
  - i.e. the object does not "own" the function.
  - Despite `this` sometimes refering to the object that was used to call the function (Ex: `obj.funcA()`), it does not mean that the function belongs to that object. This function is not any more of a "method" than other "ordinary" functions.
    - `this` only dynamically binds at runtime. Therefore, a function only has an indirect relationship with an object.
- *In JavaScript, the terms functions and methods are used interchangeably.*
### Arrays
- Arrays store values in locations indicated by numeric indexing (non-negative integers).
- However, since arrays themselves are objects, it is possible to add properties to the array using property/key access. This does not affect the length of the array.
### Objects
#### Property Descriptor
- An object consists of properties (key-value pairs) and each property has a **descriptor**, which contains the property value and other meta data describing the property.
- Example
  ```js
  const tom = { name: "Tom", age: 3 };
  Object.getOwnPropertyDescriptor(tom, "name"); // { value: 'Tom', writable: true, enumerable: true, configurable: true }
  ```
    - The meta properties (`writable`, `enumerable`, `configurable`) determines the behavior of the `name` property (whether or not it is writable, enumerable, configurable).
      - `writable` - whether or not the property's value is writable (if `false`, cannot edit value).
      - `configurable` - whether or not we can use `Object.defineProperty()` to, henceforth, change the property's descriptors (not the value).
      - `enumerable` - whether to expose/reveal the property when, for example, looping over the object properties.
    - Use `Object.defineProperty(objName, propertyName, descriptor)` to add a new descriptor property or edit an existing one (if `configurable` is set to `true`).
#### Accessing Object Properties
##### `[[Get]]`
```js
const obj = {
  name: "tom"
};

obj.name; // "tom"
```
- Technically speaking, `obj.name` does not mean it looks for `name` in `obj`. Rather, it calls a `[[Get]]` operation (kind of like a function call `[[Get]]()`) on `obj`.
#### `[[Put]]`
- When executed, `[[Put]]` undergoes a few steps.
  1) Is the property an **Accessor Descriptor**? If so, call the **Setter**.
  2) If the peroperty's `writable` is `false`, silently fail (unstrict mode) or return `TypeError` (strict mode).
  3) Set the value to the property.
#### Getters and Setters
- A **Getter** is a property that calls a hidden function to "get" a value on a *property-level* (not an object-level).
- A **Setter** is a property that calls a hidden function go "set" a value on a *property-level* (not an object-level).
- Getters and Setters override the default `[[Get]]` and `[[Put]]` operations, respectively, on a property-level.
- A property that is designated as either a Getter or Setter or both is called an **Accessor Descriptor**.
  - In Accessor Descriptors, the property value and `writable` are ignored, but `configurable`, `enumerable` and the property's Get/Set is important.
  - Example
    ```js
    // Literal Syntax
    const obj = {
      get a() { // define getter for "a"
        return "blah";
      }
    }
    
    // Imperative Syntax
    Object.defineProperty(
      obj, // Target Object
      "b", // Property Name
      { // Accessor Descriptior
        get: function() { return this.a + "!!!!!" }, // define getter for "b"
        enumerable: true // "b" is an object property for certain.
      }
    );
    
    // Test
    obj; // {}
    obj.a; // "blah"
    obj.b; // "blah!!!!!"
    
    obj.a = "iama"; // cannot change since there is no setter (plus the getter is hardcoded to return "blah").
    obj.a; // "blah"
    ```

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
### Shorthand for Adding Methods to Objects
#### Example
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

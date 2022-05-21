# Data Types, Immutability, and Variable Assignment

## Table of Contents
- [Data Types](#data-types)
- [Immutability](#immutability)
- [Variable Assignment](#variable-assignment)

## Data Types
- Every variable that is created, is stored in the JavaScript Engine's memory.
### Value Type (Primitive Values)
- When we create a variable to store a ***primitive value***, we are creating a value type variable.
  - *When storing a value type variable, the actual value is stored.*
- Ex: String, Number, Boolean, undefined, null, etc.
#### Example
```js
let fruit = "orange"; // variable `fruit` points to "orange".
let color = fruit; // variable `color` points to what `fruit` is currently pointing to ("orange").

console.log(fruit); // "orange"
console.log(color); // "orange"

fruit = "watermelon"; // variable `fruit` has been reassigned to "watermelon".

console.log(fruit); // "watermelon"
console.log(color); // "orange"
```
### Reference Types (Objects)
- When we create a variable to store ***objects***, we are creating a reference type variable.
  - *When storing a reference type variable, the reference to that object is stored.*
- Ex: arrays, object literals, etc.
#### Example
```js
const veggies = ["cucumber", "carrot", "onion"]; // variable `veggies` holds the reference to the array in memory.
const groceryList = veggies; // variable `groceryList` holds the same reference to the array.

console.log(veggies); // ["cucumber", "carrot", "onion"]
console.log(groceryList); // ["cucumber", "carrot", "onion"]

veggies.push("spinach"); // "spinach" has been added to the array that both variables reference to.

console.log(veggies); // ["cucumber", "carrot", "onion", "spinach"]
console.log(groceryList); // ["cucumber", "carrot", "onion", "spinach"]
```

## Immutability
> A mutable object is an object whose state can be modified or changed over time. An immutable object, on the other hand, is an object whose state cannot be modified after it is created. | DZone Web Dev
### Immutable Data Types
- Primitive values are assigned by value and not by reference. Therefore, they are immutable.
#### Numbers
##### Example 
- 7's value cannot be changed to 8 but the value 7 stored in a variable can be changed to 8.
```js
let x = 7;
x += 1;
console.log(x); // 8
```
#### Strings
##### Example
- The value `"I am immutable!"` cannot be mutated. But the value that `str` is referencing can be reassigned to another value.
```js
let str = "I am immutable!";

str[15] = "!!!!!!!!!";
console.log(str) // "I am immutable!" // The original value in memory that str was pointing to.

str += "!!!!!!!!"; // 
console.log(str); // "I am immutable!!!!!!!!!" // This is not the same value in memory that str was pointing to. It is a whole new value created, and the variable str has changed its pointer from the original value ("I am immutable!") to this value ("I am immutable!!!!!!!!!").
```
### Mutable Data Types
- Objects are assigned by reference and not by value. Therefore, their contents are mutable.
#### Objects
##### Example
- Any change in the property of `x` is reflected in the value of `y` because they share the same reference.
- So, when updating the value of a property of `x`, the value for all the references to that object is modified.
```js
let x = { foo: "bar" };
let y = x;

x.foo = "Something Else";

console.log(x.foo); // "Something Else"
console.log(y.foo); // "Something Else"
console.log(x === y); // true
```
#### Arrays
##### Example
- Both `x` and `y` reference to the same item, and the method `push` mutates the original array.
```js
let x = ["foo"];
let y = x.push("bar");

console.log(y); // ["foo", "bar"]
console.log(x); // ["foo", "bar"]
console.log(x === y); // true
```
##### Array Mutating Methods
- `.pop`
- `.push`
- `.shift`
- `.unshift`
- `.sort`
- `.splice`
- `.reverse`
##### Array Immutating Methods
- Methods that create a new array without mutating the original array.
- `.map`
- `.filter`

## Variable Assignment
### `const`
> This declaration creates a constant whose scope can be either global or local to the block in which it is declared. | MDN
- It creates a ***read-only reference*** to to a value.
  - **This means that the variable identifier cannot be reassigned.**
    - i.e. the value of the constant (whether primitive or reference type) cannot be changed through reassignment.
    - Example
      ```js
      const a = "hello";
      a = "goodbye"; // Uncaught TypeError: Assignment to constant variable.
      
      const b = [1,2,3];
      b = [4,5,6]; // Uncaught TypeError: Assignment to constant variable.
      ```
  - **It does not mean that the value being held is immutable.**
    - The contents of an object (ex: array, object literal) can be changed because doing so does not reassign or redeclare the constant, it simply updates the object that the constant is pointing to.
    - Unlike objects, for primitive values, the actual value (not their reference) is stored in memory. Therefore, those constants cannot be mutated.
      - Example
        ```js
        // Reference Type
        const c = [1,2];
        c.push(3);
        
        console.log(c); // [1,2,3]
        
        // Value Types
        const n = 5;
        n = 2; // Uncaught TypeError: Assignment to constant variable.
        n += 2; // Uncaught TypeError: Assignment to constant variable.
        
        const str = "hello";
        str[0] = "c";
        console.log(str); // "hello"
        ```
- Since its reference to the value cannot be changed, its value must be specified as it is being declared.
  - Example
    ```js
    const a; // Uncaught SyntaxError: Missing initializer in const declaration
    ```
- When declared globally, they do not become properties of the `window` object.
  - Example
    ```js
    const a = "I am a variable in the global scope";
    this.a; // undefined
    ```
- `const` declarations are block-scoped.
- `const` declarations are not hoisted.

### `let`
> The `let` statement declares a block-scoped local variable, optionally initializing it to a value. | MDN
- When declared globally, they do not become properties of the `window` object.
  - Example
    ```js
    let a = "I am a variable in the global scope";
    this.a; // undefined
    ```
- `let` declarations are block-scoped.
- `let` declarations are not hoisted.

### `var`
> The `var` statement declares a function-scoped or globally-scoped variable, optionally initializing it to a value. | MDN
- When declared globally, they become properties of the `window` object.
  - Example
    ```js
    var a = "I am a variable in the global scope";
    this.a; // "hello"
    ```
- `var` declarations are function-scoped.
  - They are scoped to their current execution context and closures thereof.
- `var` declarations are hoisted.
  - i.e. before any code is executed in the script, `var` declarations are processed first (initialized to `undefined`).

## Reference
[Immutability in JavaScript — When and Why Should You Use It - DZone Web Dev](https://dzone.com/articles/immutability-in-javascriptwhy-and-when-should-you#:~:text=A%20mutable%20object%20is%20an,modified%20after%20it%20is%20created.&text=In%20JavaScript%2C%20string%20and%20numbers%20are%20immutable%20data%20types.)  
[Understanding Immutability in JavaScript | CSS-Tricks](https://css-tricks.com/understanding-immutability-in-javascript/)  
[const - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)  
[let - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz)  
[var - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)  

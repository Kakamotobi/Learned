# Programming Paradigms

## Table of Contents
- [What are Programming Paradigms?](#what-are-programming-paradigms)
- [Declarative vs Imperative Programming](#declarative-vs-imperative-programming)
- [Functional Programming vs Object-Oriented Programming](#functional-programming-vs-object-oriented-programming)

## What are Programming Paradigms?
- A style or a "way" of programming.
- Approaches to solve problems using some programming language.
- There are lots of programming languages but all of them need to follow some strategy when they are implemented and this methodology/strategy is paradigms.
- Some features that determine a programming paradigm are:
  - Modularity, objects, interrupts or events, control flow, etc.

## Declarative vs Imperative Programming
### Declarative Programming
- A type of programming paradigm that describes what programs to be executed.
- ***Declarative programs abstract the flow control process, and instead spend lines of code describing the data flow: What to do.***
- Concerned with the answer that is received more than the process.
- Focuses on end result.
- i.e. "*What* is to be done."
- Ex: functional programming, logic programming.
#### Example
```js
const doubleMap = numbers => numbers.map(n => n * 2);

console.log(doubleMap([2, 3, 4])); // [4, 6, 8]
```
  - The flow control is abstracted away by using the functional `Array.prototype.map()` utility, which allows you to more clearly express the flow of data.
### Imperative Programming
- A type of programming paradigm that describes how the program executes.
- ***Imperative programs spend lines of code describing the specific steps used to achieve the desired results — the flow control: How to do things.***
- Concerned with how to get an answer step by step.
- The order of execution is very important.
- i.e. "*How* it is to be done."
- Ex: object-oriented programming, procedural programming.
#### Example
```js
const doubleMap = numbers => {
  const doubled = [];
  for (let i = 0; i < numbers.length; i++) {
    doubled.push(numbers[i] * 2);
  }
  return doubled;
};

console.log(doubleMap([2, 3, 4])); // [4, 6, 8]
```
  - The flow control is explicitly expressed in step by step order.
### Reference
[Difference Between Imperative and Declarative Programming - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-imperative-and-declarative-programming/)

## Functional Programming vs Object-Oriented Programming
### Functional Programming (FP)
> A programming paradigm in which we try to bind everything in pure mathematical functions style.
- **Emphasizes on the use of functions where each function performs a specific task.**
- Avoid shared state, mutable data, and side-effects.
- Data in the functions are immutable.
  - Outputs of a function are purely based on the arguments. So, given the same inputs, the output will always be the same.
- Cannot hide data.
- Fundamental elements: variables and functions.
- It is a *declarative* type of programming style.
#### Core Concepts
- **Pure Function**
  - ***A function which, given the same inputs, always returns the same output, and has no side-effects.***
    - For example, `Math.max(2,8,6)` can be replaced with `8`.
    - However, if there are other side-effects (ex: saving the value to disk, logging to console), it cannot be replaced with `8`.
    - `Math.random()` is impure as it is designed to return a random value every time it is called.
  - They are the simplest reusable building blocks of code in a program.
  - *A giveaway that a function is impure is if it makes sense to call it without using its return value.*
  - Pure functions are completely independent of outside state
- **Avoid Shared State**
  - ***Shared state is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes.***
  - Avoid mutating shared state. Instead, rely on immutable data structures and pure calculations to derive new data from existing data.
  - **Example - impure version (mutating shared state)**
    ```js
    // impure version

    const addToCart = (cart, item, quantity) => {
      cart.items.push({
        item,
        quantity
      });
      return cart;
    };
    ```
      - Input: cart, item, quantity.
      - Output: the same cart with the item added to it.
      - A shared state has been mutated in this function. Other functions may be relying on that cart object state to be what it was before this function was called. Now that this shared state has been mutated, we have to worry about the impact it will have on the program logic if we change the order in which functions have been called.
  - **Example - pure version (not mutating the original shared state)**
    ```js
    // pure version
    
    const addToCart = (cart, item, quantity) => {
      const newCart = lodash.cloneDeep(cart);

      newCart.items.push({
        item,
        quantity
      });
      return newCart;

    };
    ```
      - Input: cart, item, quantity.
      - Output: a new cart with the item added to it (the original hasn't been mutated).
- **Immutability**
  - ***An immutable object is an object that cannot be modified after it is created.***
- **Side Effects**
  - ***Any application state change that is observable outside the called function other than its return value.***
  - Ex: modifying an external variable or object property, logging to console, triggering any external process, calling any other functions with side-effects.
### Object-Oriented Programming (OOP)
> A programming paradigm that relies on the concept of **classes** and **objects**.
- **Structures a program into simple, reusable pieces of code (usually classes), which are used to create individual instances of objects.**
- Objects are used to represent things.
- Data used are mutable data.
  - Data (held as attributes in objects) are manipulated using methods given to the object.
- Can hide data through encapsulation.
- Fundamental elements: objects and methods.
- It is an *imperative* type of programming style.
### Reference
[Difference between Functional Programming and Object-oriented Programming - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-functional-programming-and-object-oriented-programming/)  
[Functional Programming VS Object Oriented Programming - Medium](https://medium.com/@shaistha24/functional-programming-vs-object-oriented-programming-oop-which-is-better-82172e53a526)  
[Master the JavaScript Interview: What is Functional Programming? - Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0) 


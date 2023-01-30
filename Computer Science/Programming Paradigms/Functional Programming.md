## Functional Programming (FP)

## Table of Contents
- [What is Functional Programming(FP)?](#what-is-functional-programmingfp)
- [Core Concepts](#core-concepts)
  - [Pure Function](#pure-function)
  - [Avoiding Side Effects](#avoiding-side-effects)
  - [Avoiding Shared State](#avoiding-shared-state)
  - [Immutable Data](#immutable-data)
- [Pros and Cons of FP](#pros-and-cons-of-fp)

## What is Functional Programming(FP)?
> A programming paradigm in which we try to bind everything in pure mathematical functions style.
- **Emphasizes on the use of functions where each function performs a specific task.**
- Avoid shared state, mutable data, and side effects.
- Data in the functions are immutable.
  - Outputs of a function are purely based on the arguments. So, given the same inputs, the output will always be the same.
- Cannot hide data.
- Fundamental elements: variables and functions.
- It is a *declarative* type of programming style.

## Core Concepts
### Pure Function
- **A function which, given the same inputs, always returns the same output, and produces no side effects.**
- If there are side effects (Ex: saving the value to disk, logging to console), the function is impure as the inputs cannot be guaranteed to map to the current output.
  - *A giveaway that a function is impure is if it makes sense to call it without using its return value.*
- Pure Functions are the simplest reusable building blocks of code in a program.
- Pure functions are completely independent of outside state and are all about mapping inputs to outputs.
- Examples
  ```js
  // Pure Function
  Math.max(2,8,6); // 8 - Math.max(2,8,6) is mapped to 8.

  // Impure Function
  Math.random(); // Cannot be mapped to a single output.
  ```
  ```js
  // Pure Function
  const double = (x) => {
    return x * 2;
  }

  double(3); // 6 - double(3) is mapped to 6.


  // Impure Function
  const double = (x) => {
    console.log(x); // Side Effect - the console object's state is altered.
    return x * 2;
  }

  double(3); // double(3) cannot simply be mapped to 6 as it produced a side effect.
  ```
#### Pure Functions in SPAs
- React and Redux.
  - Redux stores all the state for the app.
  - Reducers are provided, which are responsible for updating the state.
    - ***Reducers must be Pure Functions.***
  - Action Objects are passed in to the Reducers to indicate what state needs to be changed and to what value it should be changed to.
  - The same sequence of actions will always produce the same result but, it relies on pure functions.
### Avoiding Side Effects
- **A Side Effect is any application state change that is observable outside the called function other than its return value.**
- Ex: modifying an external variable or object property, logging to console (changing console object state), triggering any external process, calling any other functions with side-effects.
- Examples
  ```js
  let num = 123;

  const addToNum = (newVal) => {
    return num + newVal; // Side Effect - depending on external code (num).
  }
  ```
  ```js
  let state = 0;
  let num = 123;

  const addToNum = (newVal) => {
    state = num + newVal; // Side Effect - modifying external code.
    return num + newVal;
  }
  ```
### Avoiding Shared State
- **Shared state is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes.**
- Avoid mutating shared state. Instead, rely on immutable data structures and pure calculations to derive new data from existing data.
- **Example - Impure Version (mutating shared state)**
  ```js
  // Impure Version

  const addToCart = (cart, item, quantity) => {
    cart.items.push({
      item,
      quantity
    });

    return cart;
  };
  ```
    - Inputs: `cart`, `item`, `quantity`.
    - Output: the same `cart` with the `item` and `quantity` added to it.
    - A shared state (`cart`) has been mutated in this function. Other functions may be relying on that `cart` object state to be what it was before this function was called. Now that this shared state has been mutated, we have to worry about the impact it will have on the program logic if we change the order in which functions have been called.
- **Example - Pure Version (not mutating the original shared state)**
  ```js
  // Pure Version

  const addToCart = (cart, item, quantity) => {
    const newCart = lodash.cloneDeep(cart);

    newCart.items.push({
      item,
      quantity
    });

    return newCart;
  };
  ```
    - Inputs: `cart`, `item`, `quantity`.
    - Output: a new cart object with the item added to it (the original hasn't been mutated).
### Immutable Data
- **Functional code should be stateless. This means that when data is created, it is never mutated.**
- In JS, object/array passed in as arguments are references to that object/array and not a standalone. Therefore, making changes to it in the function causes a mutation to the original object/array, which is a side effect.
  - *Obviously, data has to change. But we have to do it without mutating the original.*
    - This is typically done by using [Higher Order Functions](https://github.com/Kakamotobi/Learned/blob/main/JS/Functions.md#higher-order-functions).
    ```js
    const original = [1, 2, 3];

    // Impure Function
    const doubleArrVals = (arr) => {
      arr.forEach((val, idx, arr) => arr[idx] = val * 2); // Side Effect - mutating original array
    }

    doubleArrVals(original);
    console.log(original); // [2, 4, 6]
    ```
    ```js
    const original = [1, 2, 3];
    
    // Pure Function
    const doubleArrVals = (arr) => {
      return arr.map(val => val * 2); // Higher Order Function
    }

    doubleArrVals(arr);
    console.log(original); // [1, 2, 3]
    ```

## Pros and Cons of FP
### Pros
- Code Clarity
  - Code is easier to read, maintain, debug, and test since functions are small, well-defined and focused on one thing.
- Immutable Data
  - Not allowing mutation of the original data allows for reduced possibilities of bugs.
- Concurrency
  - Easier to write concurrent programs because since data is immutable and there are no side effects, the state of data cannot be mutated by multiple threads of execution.
  - i.e. threads are able to work concurrently because data is not shared between them.
- Reusability
  - Higher order functions make it easier to abstract and reuse code.
### Cons
- Performance
  - Data being immutable means that many new objects may have to be created, which takes up memory.
- Verbosity
  - May be hard to understand code since there can be many nested functions and abstractions.
- Modeling Real-World Scenarios
  - It is difficult to model real-world scenarios (Ex: mutable state, side effects).

## Reference
[Difference between Functional Programming and Object-oriented Programming - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-functional-programming-and-object-oriented-programming/)  
[Functional Programming VS Object Oriented Programming - Medium](https://medium.com/@shaistha24/functional-programming-vs-object-oriented-programming-oop-which-is-better-82172e53a526)  
[Master the JavaScript Interview: What is a Pure Function? | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)  
[Master the JavaScript Interview: What is Functional Programming? | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)  

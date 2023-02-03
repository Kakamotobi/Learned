# Functional Programming (FP)

## Table of Contents
- [What is Functional Programming(FP)?](#what-is-functional-programmingfp)
- [Core Concepts](#core-concepts)
  - [Pure Function](#pure-function)
  - [Avoiding Side Effects](#avoiding-side-effects)
  - [Avoiding Shared State](#avoiding-shared-state)
  - [Immutable Data](#immutable-data)
- [Function (mathematical)](#function-mathematical)
- [Function Composition](#function-composition)
  - [Function Composition and Currying](#function-composition-and-currying)
  - [Chaining](#chaining)
  - [Pipe](#pipe)
  - [Higher Order Function](#higher-order-function)
- [Currying](#currying)
- [Partial Application](#partial-application)
- [Lazy Evaluation](#lazy-evaluation)
- [Category Theory and Monad](#category-theory-and-monad)
  - [What is Category Theory?](#what-is-category-theory)
  - [Category Theory in the Context of Programming](#category-theory-in-the-context-of-programming)
  - [What is a Monad?](#what-is-a-monad)
    - [Monad in the Context of Programming](#monad-in-the-context-of-programming)
  - [What is the Point of Monads?](#what-is-the-point-of-monads)
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

## Function (mathematical)
### Traditional Function
- A function is like a vending machine.
- *f(x) = y*
  - *x* is essentially a set of inputs.
  - *y* is essentially a set of outputs.
  - By definition, each unique element in *x* always maps to a unique element in *y*.
    - *i.e. no input or output is left hanging since each and every input is mapped to a single corresponding output. This means no input maps to more than one output, and vice versa.*
    - Ex: *y = x \* 2* never outputs two *y*s.
- *f(x,y) = x + y*
  - Function with multiple arguments.
  - These multiple arguments is combined to be considered as one set.
### Expanded Function
- *f(x) = y<sub>1</sub> | y<sub>2</sub> | y<sub>3</sub>*
  - Function that has multiple outputs for a given input.
  - Example
    - *f(x) = x<sup>2</sup>*
      - *f(2) = 4 = y*
      - *f(-2) = 4 = y*
    - The inverse of the above function would mean that there are two different outputs (*2* and *-2*) for a single input (4).
      - *sqrt(4) = ±2*

## Function Composition
> ...**function composition** is an operation (*⚬*) that takes two functions *f* and *g*, and produces a function *h = g ⚬ f* such that *h(x) = g(f(x))*. | Wikipedia

- Function Composition refers to combining two or more functions to form a new function.
  - i.e. one function is applied to the result of another function.
  - i.e. the output of one function becomes the input to another function.
  - Ex: `g(f(x))` is a composition of the two functions `g` and `f`. This composition is a new function that maps an input to `g(f(x))`.
- Functional Programming is all about Function Composition.
### Function Composition and Currying
- Currying can be used to enable function composition.
- Since currying is converting a function with multiple arguments into a sequence of functions with single arguments, the intermediate result of each function is passed on to the next function, which is function composition.
- Ex: `curriedFunc(a)(b)(c)`
  - In this function, `curriedFunc(a)` should return a function or closure that accepts the parameter `b`, which then accepts parameter `c`.
### Chaining
```js
const res = [2,1,3]
  .sort((a,b) => a - b)
  .map((x) => x * 2)
  .filter((x) => x === 2);
```
### Pipe
- **Pipe is a form of function composition that allows chaining functions together, whereby the output of the preceding function becomes the input for the next function.**
  - i.e. composing multiple simple and reusable functions together to result a more complex function that is clear and readable.
- Data flows in one direction only.
#### Example
- Instead of:
  ```js
  let res = minus3(double(add5(0))); // 7
  ```
- Use a pipe:
  ```js
  const pipe = (...fns) => initVal => {
    return fns.reduce((res, fn) => {
      return fn(res);
    }, initVal);
  };
  
  let res = pipe(add5, double, minus3)(0); // 7
  ```
  - This `pipe` function is curried so that `initVal` can be passed on to the inner function.
    - The function that is returned from `pipe` is a closure as it can access the variables `fns`, which are in `pipe`.
  - The functions `fns` should be pure functions.
### Higher Order Function
- **A Higher Order Function is a form of function composition where a function takes a function as an argument OR returns a function.**
- Example
  ```js
  const hof = (fn1, fn2) => (x) => fn1(fn2(x));
  ```
#### What is the Point of Higher Order Functions?
- The point is to abstract operations to allow greater flexibility, modularity, and code reusability.
- Common Use Cases
  - Function Composition
    - Multiple functions are combined into a new function to perform complex operations.
  - Map and Filter Operations
  - Callback Functions
  - Creating Closures
#### Taking a Function as an Argument
- JavaScript has built-in higher order functions that take functions as arguments.
- For example, `Array.prototype.map()`, `Array.prototype.filter()` calls the received function on each element and returns a new array.
##### Example
```js
let nums = [1, 2, 3];

const double = (arr) => {
  const doubled = [];
  for (let el of arr) {
    doubled.push(el * 2);
  }
  return doubled;
}

console.log(double(nums)); // [2, 4, 6]
console.log(nums); // [1, 2, 3]

// OR

nums.map(el => el * 2);
```
#### Returning a Function
- Higher Order Functions can serve as "function factories" that "distribute" functions.
- Useful for when you want to start with some base functionality and then extend it with some dynamic data.
##### Examples
###### Example 1
```js
const calculate = (operation) => {
  switch (operation) {
    case "ADD":
      return function (a, b) {
        return a + b;
      };
    case "SUBTRACT":
      return function (a, b) {
        return a - b;
      };
    case "MULTIPLY":
      return function (a, b) {
        return a * b;
      }
    case "DIVIDE":
      return function (a, b) {
        return a / b;
      }
  }
}

const addition = calculate("ADD");
addition(3, 5); // 8

// OR
calculate("ADD")(3, 5); // 8
```
- Invoking `calculate("ADD")` returns an anonymous function under the case `"ADD"`, which we store in `addition`.
- Now, we can invoke the return function by calling `addition()` with the required arguments. 
###### Example 2
- Assume we need to append strings with emojis.
```js
// This base function can be used to compose more complex functions.
const appendEmoji = (fixed) => {
  return (dynamic) => fixed + dynamic; // The inner function takes both arguments and adds them together.
}

// Use the base function to create more specialized functions that point to a specific emoji.
const rain = appendEmoji("🌧");
const sun = appendEmoji("🌞");

console.log(rain(" today"); // 🌧 today
console.log(sun(" tomorrow"); // 🌞 tomorrow
```
- This code does not rely on any shared state.

## Currying
> ...currying is the technique of translating the evaluation of a function that takes multiple arguments into evaluating a sequence of functions, each with a single argument. | Wikipedia

- **The idea of Currying is to take a function that takes multiple arguments and break it down into a sequence/chain of functions that each take only one argument.**
  - i.e. curried functions take multiple arguments one at a time and return a new function for each argument.
### What Problem is Currying Trying to Solve?
- To allow creating more specialized functions from an existing function. Thereby, reusing code in new contexts.
  - i.e. ***curried functions essentially return containers (functions) that all share the same interface.***
    - Instead of dealing with the values inside of containers, currying is about dealing with containers, which allows the creation of generic functions that act as the common interface.
- The most common use case for currying is **function composition**.
  - Function Composition is when the return value of one function is passed into another function as an argument.
  - Ex: `c(x) = f(g(x))`.
### Example
```js
// Ordinary function

const multiply = (x, y) => x * y;
multiply(3, 2); // 6
```
```js
// Curried function

const multiply = (x) => (y) => x * y;
multiply(3)(2);

const double = multiply(2);
double(5); // 10
const triple = multiply(3);
triple(5); // 15
const quadruple = multiply(4);
quadruple(5); // 20
```
- The function `multiply` is *partially applied* to the argument `x`.
  - "Partially applied" because `x` is fixed in the inner function to be returned.
- `multiply` is the curried function with a single parameter `x` that returns a new function with a single parameter `y`.
- `multiply(x)` returns a partially applied version of the `multiply` function that is waiting for argument `y`.
### Relationship with Partial Application
- A curried function partially applies one argument at a time.
- A curried function does not have to invoke an application right away.
  - Example
    ```js
    const double = multiply(2);
    doSomething().then(() => double(5));
    ```

## Partial Application
> The process of applying a function to some of its arguments. The partially applied function gets returned for later use. | Eric Elliott

- Here, **Application** refers to applying a function to its arguments.
- **The idea of Partial Application is a function receiving a function with multiple parameters and returning a function with fewer parameters.**
  - i.e. some arguments of the "higher" function are fixed inside the returned function, and the remaining arguments are passed on to the returned function as arguments.

## Lazy Evaluation
- **Lazy Evaluation** refers to delaying evaluation of an operation until it is actually needed.
  - i.e. a function executes its statement and evaluates the needed values as they are needed.
  - Example
    ```js
    // The arguments `supplier1` and `supplier2` are not evaluated until needed (Ex: `supplier1.get()`).
    // So, if `supplier1.get()` evaluates to false, `supplier2.get()` is never evaluated.
    const lazyFunc = (supplier1, supplier2) => {
      return supplier1.get() && supplier2.get() ? "success" : "failure";
    };
    ```
- cf. **Eager Evaluation**
  - Refers to evaluating an operation as soon as it is encountered.
    - i.e. a function first evaluates its arguments and then executes its statement.
  - The larger the data that is being passed on, the more memory is used.
  - Example
    ```js
    // The arguments `res1` and `res2` are evaluated first before running the statement.
    const eagerFunc = (res1, res2) => {
      return res1 && res2 ? "success" : "failure";
    };
    ```

## Category Theory and Monad
### What is Category Theory?
- A general mathematical theory of how different structures are related to one another.
- _Category Theory is about defining categories and ways to morph one category to another._

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ef/Commutative_diagram_for_morphism.svg/200px-Commutative_diagram_for_morphism.svg.png" alt="Category Theory" width="30%"/>
</p>

- _X_, _Y_, _Z_ represent **categories**.
- _f_, _g_, _g_ ∘ _f_ represent **functors**.
  - A **Functor** is a structure-preserving mapping between categories.
    - The term "functor" was introduced to distinguish itself from other functions.
    - Examples
      - The value has changed but the input and output of the function is the same.
        ```js
        [1, 2, 3].map((x) => x * 2); // [2,4,6]
        ```
        ```
        Type Array --> Function --> Type Array
        ```
      - If the input and output are immutable and the function is pure, it is possible to chain.
        ```js
        [1, 2, 3]
          .map(double) // [2,4,6]
          .map(double) // [4,8,12]
          .map(double); // [8,16,24]
        ```
        ```
        Input --> Pure Function --> Output --> Pure Function --> Output --> Pure Function --> Output
        ```
  - A **Morphism** is a structure-preserving mapping between objects of the same type in a category.
### Category Theory in the Context of Programming
- In the context of Programming, "category" is equivalent to "type".
- **A functor in functional programming is essentially a mapping function that converts an instance of Type _X_ to Type _Y_.**
### What is a Monad?
- a.k.a. endofunctor.
- **A Monad is a Functor that outputs the same category as before.**
  - In the context of Functional Programming, a Monadic operator is an operator or function that changes the value but maintains the same type as before.
    - The term "monadic operator" is used because this process is similar to a monad.
    - A Monadic Type wraps the value in its type and hence, allows the value to be handled as that type.
      - Ex: `Maybe` in Haskell and `Optional` in Swift are Monadic types.
#### Intro
- The building block of a functional program is a function.
  - Each function is simply a mapping between the input parameter's type and that of the output.
    - `func: TypeA -> TypeB`
- How are we going to compose a whole program/application based on this building block?
  - One answer is Monads.
#### Monad in the Context of Programming
- **Monad is a design pattern in which pipeline implementation details are abstracted by wrapping a value in a type.**
  - i.e. a Monad is a wrapper around a value that adds additional behavior or operations to that value. It provides a way to chain these operations together in a sequence (Ex: through binding), and abstracts the implementation details.
  - Monads are object instances. Therefore, they can store some data (Ex: value, history, etc.).
  - Analogy: a smart box that does stuff to its contents for us.
- _Monad is used in order to **guarantee some stability in terms of types**._
  - When an input is mapped to an output, the mapping has to be to the same type.
    - i.e. when the same type as the input is returned, it is considered stable.
  - Therefore, even if the result is `null` or `undefined`, the same type should be outputted.
- Haskell provides a `Maybe` Monad that can be used along with a bind operator (`>>=`), which removes the `.bind()` clutter.
##### [Example](https://www.youtube.com/watch?v=VgA4wCaxp-Q&ab_channel=AByteofCode) - Maybe Monad
- Trying to get the age of the first friend of the user "Kakamotobi".
  ```js
  let username = "Kakamotobi";
  let userObj = database.fetch(username);
  if (userObj !== null) {
    let userFriends = userObject.friends;
    if (userFriends !== null) {
      let firstFriendObj = userFriends[0];
      if (firstFriendObj !== null) {
        let firstFriendAge = firstFriendObj.age;
      }
    }
  }
  ```
  - Any one of these operations could fail (Ex: database failure, `userFriends` is empty, etc.). To handle this, you would need a lot of failure checking with numerous `if` statements, which makes the code verbose, hard to read, and focused more on implementation details rather than the actual objective.
- Create a `Maybe` class that is simply composed of two things: a value, and a binding method.

  ```js
  class Maybe {
    constructor(val) {
      this.val = val;
    }

    // Simply apply `fn` to `this.val` and return a new instance of Maybe with the newly computed value.
    bind(fn) {
      if (this.val === null) return this;
      val = fn(this.val);
      return new Maybe(val);
    }
  }
  ```
  ```js
  let firstFriendAge = new Maybe("Kakamotobi")
    .bind(database.fetch) // a new `Maybe` with `this.val` set to the result of DB fetch.
    .bind((userObj) => userObj.friends) // a new `Maybe` with `this.val` set to the friends list.
    .bind((userFriends) => userFriends[0]) // a new `Maybe` with `this.val` set to the first friend in the list.
    .bind((firstFriend) => firstFriend.age); // a new `Maybe` with `this.val` set to the first friend's age.
  ```
  - At this point, the only location where a function is being called is in the definition of the `bind` function. Changes only need to be made in here.
##### Example - Monad Class
```js
class Monad {
  constructor(val, log = null) {
    this.val = val;
    // Log that keeps track of every step of the pipeline.
    // This simulates a sort of mutable state, but still only using immutable data.
    this.log = log ?? [];
  }

  apply(fn) {
    val = fn(this.val);
    return Monad(val, [...this.log, val]);
  }
}
```
#### What is the Point of Monads?
- Monads provide a uniform framework for thinking about programming with effects.
- Support pure programming.
- Use of effects is explicit in types (Ex: `Maybe` explicitly indicates what kind of side effects that the program could have).
- Writing functions that work for any effect (effect polymorphism).
  - Ex: to run a sequence of effects one after the other, you could write a generic function that receives a sequence of effects of any type and run them.

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
[Function composition - Wikipedia](https://en.wikipedia.org/wiki/Function_composition)  
[함수형 프로그래밍 — Pipe. 파이프란 무엇이고 어떻게 사용하는가? | by Moon | 오늘의 프로그래밍 | Medium](https://medium.com/%EC%98%A4%EB%8A%98%EC%9D%98-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-pipe-c80dc7b389de)  
[Higher Order Functions (Composing Software) | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/higher-order-functions-composing-software-5365cf2cbe99)  
[Higher Order Functions in JavaScript – Beginner's Guide](https://www.freecodecamp.org/news/higher-order-functions-in-javascript/)  
[Currying - Wikipedia](https://en.wikipedia.org/wiki/Currying)  
[javascript - What is 'Currying'? - Stack Overflow](https://stackoverflow.com/questions/36314/what-is-currying)  
[Curry or Partial Application?. The Difference Between
Partial… | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/curry-or-partial-application-8150044c78b8)  
[Eager vs Lazy Evaluation - tutorialspoint](https://www.tutorialspoint.com/functional_programming_with_java/functional_programming_with_java_evaluation.htm)  
[Monads in JavaScript - Stack Overflow](https://stackoverflow.com/questions/11871065/monads-in-javascript)  
[What is a monad? (Design Pattern) - YouTube](https://www.youtube.com/watch?v=VgA4wCaxp-Q&ab_channel=AByteofCode)  
[Understanding Monads With JavaScript - igstan.ro](http://igstan.ro/posts/2011-05-02-understanding-monads-with-javascript.html)  

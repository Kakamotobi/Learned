# `this`

## Table of Contents
- [What Is It?](#what-is-it)
  - [Why Use It?](#why-use-it)
  - [Disclaimers](#disclaimers)
- [What is it REALLY?](#what-is-it-really)
  - [Call Site](#call-site)
  - [How the Call Site Decides the Value of `this`](#how-the-call-site-decides-the-value-of-this)
    - [Order of Priority (Most to Least)](#order-of-priority-most-to-least)
    - [1. Default Binding - Standalone Function Invocation](#1-default-binding---standalone-function-invocation)
    - [2. Implicit Binding](#2-implicit-binding)
    - [3. Explicit Binding](#3-explicit-binding)
    - [4. `new` Binding](#4-new-binding)
- [`this` and Arrow Functions](#this-and-arrow-functions)
- [Rules of Thumb](#rules-of-thumb)

## What Is It?
> A property of an execution context (global, function or eval) that, in non–strict mode, is always a reference to an object and in strict mode can be any value.

- **`this` refers to the object that is executing the current piece of code.**
  - It is an identifier that is automatically set in every function's scope.
- **`this` binds at runtime, and its context/value completely depends on the **Call Site**(point of invocation) of the function (irrelevant of where the function was declared).**
  - i.e. the value of `this` completely depends on HOW and WHERE the function was executed.
    - Ex: `printBio()` // `this` will refer to the `window` object.
    - Ex: `person.printBio()` // `this` will refer to the object called person.
### Example
```js
function capitalize() {
  return this.name.toUpperCase();
}

capitalize({ name: "tom" }); // "" // `this` refers to the `window` object.
capitalize.call({ name: "tom" }); // "TOM" // `this` refers to the object that was passed in.
```
### Why Use It?
- Instead of using the `this` keyword to pass along the object reference, why not just pass in context objects as arguments?
  - Because it’s neater and clearer, especially when things get more complicated.
### Disclaimers
#### `this` does NOT refer to the Function itself
- Since `this` does not refer to the function itself, we have a few options to choose from.
  ```js
  // Lexical Scope Approach 1 - avoid using `this`
  // Move the `count` property to a separate object in the lexical scope.

  const data = {
    count: 0;
  }

  function incrementCount(num) {
    data.count++;
  }

  for (let i = 0; i < 10; i++) {
    if (i > 5) incrementCount();
  }

  console.log(data.count); // 4
  ```
  ```js
  // Lexical Scope Approach 2 - avoid using `this`
  // Use the function's lexical scope.

  function incrementCount() {
    incrementCount.count++;
  }

  incrementCount.count = 0;

  for (let i = 0; i < 10; i++) {
    incrementCount();
  }

  console.log(incrementCount.count); // 4
  ```
  ```js
  // `this` Approach
  // Force the function to refer itself as the value of `this` using `.call()`.

  function incrementCount() {
    this.count++;
  }

  incrementCount.count = 0;

  for (let i = 0; i < 10; i++) {
    incrementCount.call(incrementCount);
  }

  console.log(incrementCount.count); // 4
  ```
#### `this` does NOT refer to the Lexical Scope of the Function
- `this` does not refer to the function's scope.
  - A function's scope 'object' is not accessible through JS to begin with as it is an internal part of the engine.
  - Therefore, it is not possible to use `this` to access something in the lexical scope.

## What is it REALLY?
### Call Site
- Upon invoking a function, an **Execution Context** is created. This Execution Context contains information including the call stack, invocation method, arguments that were passed in, and the `this` reference, which serves to be used during the execution of the function.
  - Therefore to understand what `this` binds to, we must look at the function's **Call Site** (invocation of the function; NOT declaration).
    - **The Call Site is inside the piece of code "right before" the currently executing function in the call stack.**
      - i.e. where the current function (in the call stack) was executed in.
      - Example
        ```js
        function funcA() {
          funcB(); // funcB's call site is the inside of funcA.
        }
        
        function funcB() {
          funcC(); // funcC's call site is the inside of funcB.
        }
        
        function funcC() {}
        
        funcA(); // funcA's call site is the inside of the global scope.
        ```
### How the Call Site Decides the Value of `this`
- After checking the Call Site, check which of the following 4 rules apply.
  - If several rules can be applied to the situation, prioritize.
#### Order of Priority (Most to Least)
- Explicit Binding --> `new` Binding --> Implicit Binding --> Default Binding
#### 1. Default Binding - Standalone Function Invocation
- **This is the default rule that is applied whenever a standalone function is invoked AND if none of the other rules apply.**
  - *i.e. if a dot(`.`), `.call()`, `.apply()`, or `.bind()` is not being used.*
- *In strict mode, the global object is excluded from default binding.*
  - Hence, binds by default to `undefined`.
- *In non-strict mode, the global object is the only thing susceptible to default binding (irrelevant of the strictness of the function's Call Site).*
  - Hence, binds by default to the `window` object.
##### Example
```js
// Default Binding Applied

function defaultBinding() {
  console.log(this.a);
}

var a = "default binding";

defaultBinding(); // "default binding" // If strict mode, undefined.
```
#### 2. Implicit Binding
- **If there is a Context Object in the Call Site that "owns"/"contains" the function reference, `this` is bound to that Context Object.**
  - i.e., when dot notation is used to invoke a function, `this` will be set to what is on the left of the dot(`.`).
##### Example
```js
function implicitBinding() {
  console.log(this.a);
}

// Whether or not the `implicitBinding` function is declared in this object or later added as a reference, `obj` does not "own"/"contain" the function. However, the `obj` context is the Call Site referencing the function. Therefore, we can say that `obj` "owns"/"contains" the function reference at the point of invocation.
const obj = {
  a: "implicit binding",
  implicitBinding: implicitBinding 
};

implicitBinding(); // undefined
obj.implicitBinding(); // "implicit binding"
```
- For chained object property references, the most Top/Last level is linked to the Call Site.
  ```js
  function implicitBinding() {
    console.log(this.a);
  }
  
  const obj2 = {
    a: "implicit binding 2",
    implicitBinding: implicitBinding
  }
  
  const obj1 = {
    a: "implicit binding 1",
    obj2: obj2
  }
  
  obj1.obj2.implicitBinding(); // "implicit binding 2" // `obj1.a` is ignored.
  ```
##### Edge Cases - Loss of Implicit Binding
- Depending on the strict mode, `this` is bound by default to either the global scope or `undefined`.
  ```js
  function implicitBinding() {
    console.log(this.a);
  }

  const obj = {
    a: "implicit binding",
    implicitBinding: implicitBinding
  }

  // `anotherReference` is yet another direct reference to `implicitBinding`.
  const anotherReference = obj.implicitBinding;

  var a = "i am in the global scope!";

  // And its Call Site is the global scope (i.e. default binding is applied so `this` refers to the `window` object).
  anotherReference(); // "i am in the global scope!"
  ```
- The binding of `this` can become "lost" when using callbacks (incl. when passing into built-in functions).
  ```js
  function implicitBinding() {
    console.log(this.a);
  }
  
  function doImplicitBinding(fn) { // `fn` is yet another direct reference to `implicitBinding`.
    fn(); // `implicitBinding`'s Call Site is `doImplicitBinding`'s scope. Therefore, `this` refers to the `window` object.
  }
  
  const obj = {
    a: "implicit binding",
    implicitBinding: implicitBinding
  }
  
  var a = "i am in the global scope!";
  
  doImplicitBinding(implicitBinding); // "i am in the global scope!"
  ```
#### 3. Explicit Binding
- **Directly binding `this` to the specified object.**
  - i.e. using `.call()`, `.apply()`, or `.bind()` on a function.
    - These methods accept an object to bind `this` to as the first argument. The following arguments are passed on to the function.
  - Unlike Implicit Binding, what is on the left of the dot notation in the function call is no longer the Context Object of `this`.
- However, the binding of `this` can also be lost or frameworks can overwrite it. [Hard Binding](#hard-binding) is a way to avoid this.
##### Example
```js
function explicitBinding() {
  console.log(this.a);
}

const obj = {
  a: "explicit binding"
}

explicitBinding.call(obj); // 2
explicitBinding(); // undefined
```
##### Hard Binding
- Wrapping the explicitly bound function call with another function.
- It is useful in cases where you need to pass in an argument and receive a return value.
###### Example
```js
function explicitBinding() {
  console.log(this.a);
}

const obj = {
  a: "explicit binding"
}

const hardBinding = function() {
  explicitBinding.call(obj); // `obj` is hard coded as `this` no matter how the `hardBinding` function is invoked.
}

hardBinding(); // "explicit binding"
setTimeout(hardbinding, 100); // "explicit binding"
hardBinding.call(window); // "explicit binding" // redefining `this` to `window` has no use.
```
```js
// ES5 `Function.prototype.bind()` Implementation
// `.bind()` returns a function that is hard coded to call the original function in the specified `this` context.

function bind(fn, obj) {
  return function() {
    return fn.apply(obj, arguments);
  }
}

const hardBoundFunc = bind(someFunc, someObj);
```
#### 4. `new` Binding
- **Using the `new` keyword to call a function binds `this` to the newly created object, which is returned by the constructor of the function that was called.**
  - Calling a function prefixed with the `new` keyword causes [these](https://github.com/Kakamotobi/Learned/blob/main/JS/new.md#the-4-things-that-new-does) to happen.
##### Example
```js
function newBinding(a) {
  this.a = a;
}

const test = new newBinding("new binding");
test.a; // "new binding"
```

## `this` and Arrow Functions
- **Arrow functions do not get their own `this` value.**
  - i.e., `this` in an arrow function will not change from the `this` of its parent or of its nearest `this` (the local execution context).
### How It Can Be Useful
- It can be better to use arrow functions when we don't want a new `this` value.
#### Example
```js
const annoyer = {
  phrases: ["literally", "cray cray", "I can't even", "Totes!", "YOLO", "Can't Stop, Won't Stop"],
  pickPhrase() {
    const { phrases } = this;
    const randIdx = Math.floor(Math.random() * phrases.length);
    return phrases[randIdx]
  }
  start() {
    this.timerId = setInterval(() => {
      console.llg(this.pickPhrase())
      , 3000)
  },
  stop() {
    clearInterval(this.timerId);
  }
}
```

## Rules of Thumb
- If the function was defined with an arrow function.
  - Write `console.log(this)` on the first valid line above the arrow function. Value of this in the arrow function will be equal to that console log.
- If the functionw as invoked using `.bind()`, `.call()`, or `.apply()`.
  - `this` is equal to the first argument of `.bind()`, `.call()`, or `.apply()`.
- All other cases
  - `this` is equal to whatever is on the left of the `.` in the method call.

## Reference
You Don't Know JS - Kyle Simpson  
[this - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)  

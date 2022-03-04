# `this`
> A property of an execution context (global, function or eval) that, in non–strict mode, is always a reference to an object and in strict mode can be any value.
- **Gives you an object referencing the *current execution scope.***
  - *Depending on the scope and rules of how `this` works, that object changes.*
- **The value of `this` depends on the invocation context of the function that it is used in.**
  - i.e., the value of `this` will change depending on how the function was executed; not just where it is written.
  - **`this` will be set to what is on the left.**
    - Ex: `person.printBio()` // `this` will refer to the object called person.
    - Ex: `printBio()` // `this` will refer to the `window` object.

## Rules of Thumb
- If the function was defined with an arrow function.
  - Write `console.log(this)` on the first valid line above the arrow function. Value of this in the arrow function will be equal to that console log.
- If the functionw as invoked using `.bind()`, `.call()`, or `.apply()`.
  - `this` is equal to the first argument of `.bind()`, `.call()`, or `.apply()`.
- All other cases
  - `this` is equal to whatever is on the left of the `.` in the method call.

## Why use the this keyword?Permalink
- Instead of using the `this` keyword to pass along the object reference, why not just pass in context objects as arguments?
- Because it’s neater and clearer, especially when things get more complicated.

## Getting common misconceptions out of the wayPermalink
- `this` does NOT refer to the function itself.
- `this` does NOT refer to the lexical scope of the function.

## So what does this refer to?Permalink
- `this` is bound only by how the function has been called; NOT to the location of the function declaration.
  - `this` is bound at the period of runtime (at the call-site); NOT when it was written.
  - The context of `this` is decided according to the moment the function was called.
- The “Activation Record” (Execution Context) is created upon calling a function.
  - It contains information on the call-stack, how the function was called, arguments passed in, and also the `this` reference.

## The Call-Site
### How was the function called?
- To find out what `this` is referring to, it is important to understand the call-site, hence the function invocation (not declaration).
- It is important to check the call stack until the point of function invocation.
- The call-site is in the code that has been called just before the currently executing function.
### How does the call-site decide what this refers to?
#### Check which of the 4 binding rules apply
##### 1. Default Binding
- `this` refers to the global window whenever a standalone function is invoked AND *if the other rules do not apply.*
  - *i.e., if a dot(`.`), `.call()`, `.apply()`, `.bind()` are not being used.*
- Ex: `foo()`
##### 2. Implicit Binding
- Whether or not there is a context object in the call-site or note; hence, whether or not the object is *owned* or *contained*.
- If there is a context object for the function reference, the context object is bound to this upon function invocation.
- i.e., when dot notation is used to invoke a function.
  - `this` will be set to what is on the left of the dot(.).
- Ex: `obj.foo()`
  - The call-site references the `foo()` function as a context of `obj`, therefore, it can be said that the `obj` object owns or contains the reference to the function at the period of invocation.
##### 3. Explicit Binding
- Using `.call()`, `.apply()`, or `.bind()` on a function.
  - These methods accept an object to bind this to as the first argument.
- *“Explicit” because we are directly binding `this` to a specified object.*
- Unlike Implicit Binding, what is on the left of the dot notation in the function call is no longer `this`.
- **Hard Binding**
  - Hard coding a specific object as `this` by passing in that object as the argument for `.call()`. Hence, that object will be `this` no matter how the function is invoked.
##### 4. `new` Binding
- When using `new`, `this` is bound to the newly created object, which is created by the constructor attached to the class.
##### Order of Priority
- Default Binding < Implicit Binding < `new` Binding < Explicit Binding

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

## Reference
You Don't Know JS - Kyle Simpson  
[this - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)  

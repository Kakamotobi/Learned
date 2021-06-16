# `this`
> A property of an execution context (global, function or eval) that, in non–strict mode, is always a reference to an object and in strict mode can be any value.
- **Gives you an object referencing the *current execution scope.***
  - *Depending on the scope and rules of how `this` works, that object changes.*
- **The value of `this` depends on the invocation context of the function that it is used in.**
  - i.e., the value of `this` will change depending on how the function was executed; not just where it is written.
  - **`this` will be set to what is on the left**
    - Ex: `person.printBio()` // `this` will refer to the object called person.
    - Ex: `printBio()` // `this` will refer to the `window` object.

## Cases
### Using `this` in a Function Declared in the Global Execution Context
- `this` refers to the `window` object.
### Using `this` in Methods (functions added to objects)
- `this` refers to the specific object that the method has been added to.
- *Allows us to access other properties or methods inside the object.*
#### Example
```
const person = {
  first: "Cherilyn",
  last: "Sarkisian",
  nickName: "Cher",
  fullName() {
    const { first, last, nickName } = this;
    return `${first} ${last} a.k.a. ${nickName}`
  },
  printBio() {
    const fullName = this.fullName();
    console.log(`${fullName} is a person!`)
  }
}
```

## `this` and Arrow Functions
- **Arrow functions do not get their own `this`.**
  - i.e., `this` in an arrow function will not change from the `this` of its parent or of its nearest `this` (the local execution context).
### How It Can Actually Be Useful
- Can be better to use arrow functions when we don't want a new `this`.
#### Example
```
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
[this - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

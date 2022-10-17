# Decorators

## Table of Contents
- [What are Decorators?](#what-are-decorators)
- [Decorators in JavaScript](#decorators-in-javascript)
  - [Class Decorators](#class-decorators)
  - [Class Member Decorators](#class-member-decorators)

## What are Decorators?
- A decorator is a way of wrapping one piece of code (Ex: a function) with another (the decorator).
  - Basically, a wrapper function.
- It is *similar* to the concept of function composition such as higher order functions.
### Why Use Decorators?
- Decorators allow us to enhance the functionality of a function without modifying the underlying function.
- Decorators are yet another means of sharing/distributing functionality across in a clean, readable way.
### Decorator Example
```js
// A function used as a decorator in Vanilla JS.

function loggerDecorator(func) {
  return function () {
    console.log("Start");
    const res = func.apply(this, args);
    console.log("End");
    return res;
  }
}

function sayHello(name) {
  console.log(`Hello, ${name}!`);
}

const loggedSayHello = loggerDecorator(sayHello); // `loggedSayHello()` is the same as `sayHello()` but with some logging.

loggedSayHello("Kakamotobi");
// Start
// Hello, Kakamotobi!
// End
```

## Decorators in JavaScript
- Decorators in JavaScript are prefixed by the `@` symbol.
- Decorators in JavaScript are yet a work in progress (Stage 3 Proposal) and can be used on a class or its members (properties, methods, getters, setters).
### Class Decorators
- Decorators that are applied to the entier class.
#### Examples
```js
const argsLogger = () => {
  return function decorator() { // decorator function
    return (...args) => {
      console.log(`Parameters: ${args}`); // log the arguments
      return new Class(...args);
    }
  }
}

@argsLogger
class Cat {
  constructor(name, age) {}
}

const tom = new Cat("tom", 3); // "tom", 3
```
```js
const withNumTails = (className) => {
  return class extends classname {
    constructor(...args) {
      super(...args);
      this.numTails = 9;
    }
    
    setNumTails(num) {
      this.numTails = num;
    }
  }
}

@withNumTails
class Cat {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const tom = new Cat("Tom", 3);
tom.numTails; // 9
tom.setNumTails(2);
```
### Class Member Decorators
- Decorators that are applied to a single member of a class.
#### Example
```js
// target: the class that the member belongs to.
// name: the name of the member.
// descriptor: the descriptor of the member (the object that would have been passed to `Object.defineProperty()`).

function readonly(target, name, descriptor) {
  descriptor.writable = false;
  return descriptor;
}

class Cat {
  @readonly
  species = "cat";
}

const tom = new Cat();
tom.species = "dog";
// TypeError: Cannot assign to read only property 'species' of object '#<Cat>'
```

## Reference
[What are decorators and how are they used in JavaScript ? - GeeksforGeeks](https://www.geeksforgeeks.org/what-are-decorators-and-how-are-they-used-in-javascript/)  
[JavaScript Decorators: What They Are and When to Use Them - SitePoint](https://www.sitepoint.com/javascript-decorators-what-they-are/)  
[tc39/proposal-decorators: Decorators for ES6 Classes](https://github.com/tc39/proposal-decorators)  
[WONISM | JavaScript Decorator 이해하기](https://wonism.github.io/what-is-decorator/)  

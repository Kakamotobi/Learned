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
  - Cannot trace the execution of the program as it runs.
- Focuses on end result.
- Think like a human.
  - i.e. "*What* is to be done."
- *Many (if not all) declarative APIs have some sort of underlying imperative implementation.*
- Ex: event handling code for a button can be encapsulated behind a button component, HTML, CSS.
- Ex: functional programming, logic programming.
#### Examples
##### Example 1
- The flow control is abstracted away by using the functional `Array.prototype.map()` utility, which allows you to more clearly express the flow of data.
```js
const doubleMap = numbers => numbers.map(n => n * 2);

console.log(doubleMap([2, 3, 4])); // [4, 6, 8]
```
##### Example 2
- Frontend frameworks (Ex: React) are declarative.
```js
let count = 0;

<button onClick={() => count++}>{count}</button>
```

### Imperative Programming
- A type of programming paradigm that describes how the program executes.
- ***Imperative programs spend lines of code describing the specific steps used to achieve the desired results — the flow control: How to do things.***
- Concerned with how to get an answer step by step.
  - Can trace the execution of the program as it runs.
- The order of execution is very important.
- Think like a computer.
  - i.e. "*How* it is to be done."
- Ex: object-oriented programming, procedural programming.
#### Examples
##### Example 1
- The flow control is explicitly expressed in step by step order.
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
##### Example 2
- Using Vanilla JS to manipulate the DOM.
```js
const btn = document.querySelector("button");
let count = 0;

btn.eventListener(() => {
  count++;
  btn.innerText = count;
});

<button>0</button>
```
### Reference
[Difference Between Imperative and Declarative Programming - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-imperative-and-declarative-programming/)  
[Imperative vs Declarative Programming - YouTube](https://www.youtube.com/watch?v=E7Fbf7R3x6I&ab_channel=uidotdev)

## Functional Programming vs Object-Oriented Programming
### Functional Programming (FP)
> A programming paradigm in which we try to bind everything in pure mathematical functions style.
- **Emphasizes on the use of functions where each function performs a specific task.**
- Avoid shared state, mutable data, and side effects.
- Data in the functions are immutable.
  - Outputs of a function are purely based on the arguments. So, given the same inputs, the output will always be the same.
- Cannot hide data.
- Fundamental elements: variables and functions.
- It is a *declarative* type of programming style.
#### Core Concepts
- **Pure Function**
  - ***A function which, given the same inputs, always returns the same output, and produces no side effects.***
  - If there are side effects (Ex: saving the value to disk, logging to console), the function is impure as the inputs cannot be guaranteed to map to the current output.
    - *A giveaway that a function is impure is if it makes sense to call it without using its return value.*
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
  - Pure Functions are the simplest reusable building blocks of code in a program.
  - Pure functions are completely independent of outside state and are all about mapping inputs to outputs.
- **Side Effects**
  - ***Any application state change that is observable outside the called function other than its return value.***
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
- **Avoid Shared State**
  - ***Shared state is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes.***
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
- **Immutable Data**
  - ***Functional code should be stateless. This means that when data is created, it is never mutated.***
  - In JS, object/array passed in as arguments are references to that object/array and not a standalone. Therefore, making changes to it in the function causes a mutation to the original object/array, which is a side effect.
    - *Obviously, data has to change. But we have to do it without mutating the original.*
      - This is typically done by using [Higher Order Functions](https://github.com/Kakamotobi/Learned/blob/main/JS/Functions.md#higher-order-functions).
      ```js
      const arr = [1, 2, 3];

      // Impure Function
      const doubleArrVals = (arr) => {
        arr.forEach((val, idx, arr) => arr[idx] = val * 2); // Side Effect - mutating original array.
      }

      doubleArrVals(arr);
      console.log(arr); // [2, 4, 6]


      // Pure Function
      const doubleArrVals = (arr) => {
        return arr.map(val => val * 2); // Higher Order Function
      }

      const doubledArr = doubleArrVals(arr);
      console.log(arr); // [1, 2, 3]
      console.log(doubledArr); // [2, 4, 6]
      ```
#### Pure Functions in SPAs
- React and Redux.
  - Redux stores all the state for the app.
  - Reducers are provided, which are responsible for updating the state.
    - ***Reducers must be Pure Functions.***
  - Action Objects are passed in to the Reducers to indicate what state needs to be changed and to what value it should be changed to.
  - The same sequence of actions will always produce the same result but, it relies on pure functions.
### Object-Oriented Programming (OOP)
> A programming paradigm that relies on the concept of **classes** and **objects**.

- **Structures a program into simple, reusable pieces of code (usually classes), which are used to create individual instances of objects.**
  - The properties on the objects are manipulated using the methods given to the object.
- It is an *imperative* type of programming style.
- OOP came to rise amidst ADT and Object-orientation advancing.
  - ADT focused on type abstraction.
    - It supports creating instances of a type.
  - Object-orientation focused on data.
    - It supports inheritance and polymorphism.
#### Building Blocks of OOP
##### Class
- A template/blueprint for creating objects.
- The common properties/methods of a class are loaded to heap memory.
##### Instance
- The individual instantiation/realization of a class that is created at runtime.
- An instance is loaded to heap memory with only properties specific to it.
- Several instances together is called a collection.
##### Object
- The term "object" in this case refers to the grouping of an instance and its class.
  - i.e. the pairing of the instance and the class.
  - Ex: if instanceA and instanceB are instances of classC, instanceA and classC are one object, and instanceB and classC are another.
##### Property
- Properties are member variables and attributes describing the state of an object.
##### Method
- Methods are member functions that represent the behaviors of an object.
#### The 4 Pillars of OOP
##### Encapsulation
> ... the packing of data and functions into one component (ex: a class) and then controlling access to that component to make a “blackbox” out of the object. | MDN

- OOP bundles/encapsulates relatable variables and methods that operate on them into an object.
- This reduces complexity and increases reusability.
##### Abstraction
> ... a way to reduce complexity and allow efficient design and implementation in complex software systems. It hides the technical complexity of systems behind simpler APIs. | MDN

- OOP provides a simpler interface for the user since the complexity of a class is less exposed.
- This reduces the impact of change.
  - Any change within will not leak to the outside.
##### Inheritance
> Data abstraction can be carried up several levels, that is, classes can have superclasses and subclasses. | MDN

- A child class extending from a base/parent class inherits all of the parent's functionality and is able to overwrite or extend it with custom behaviors it needs.
- This helps eliminate redundant code.
- _Note_
  - Be careful of deeply nested classes as it can become very difficult to debug (too much abstraction!).
###### Example
```ts
class Human {
  constructor(public name) {}
  
  sayHello() {
    return `Hello, I'm ${this.name}.`;
  }
}

class SuperHuman extends Human {
  heroName;
  
  constructor(name) {
    super(name); // Execute the code in the constructor of Human (initialize name property).
    this.heroName = `HERO ${name}`;
  }
  
  superpower() {
    return `${this.heroName} beams light off eyes!`;
  }
}

const superman = new SuperHuman("Clark Kent");
superman.superpower(); // Clark Kent beams light off his eyes!
superman.sayHello(); // Hello, I'm Clark Kent.
```
###### Composition
- Composition breaks apart the interfaces and logic into small pieces, and then builds up larger functions or objects by combining these pieces together.
  - *cf. inheritence and composition are alternatives to each other for code reusability.*
- There are multiple ways to implement composition.
  - Mixin Pattern - concatenate objects together.
    - The idea is to decouple your properties or behaviors into objects or functions that return objects. Then merge all these objects together into a final function that does everything that we need it to.
    - Basically, a certain type of multiple inheritance.
    ```js
    const getName = (name) => {
      return { name };
    };

    const canSayHello = (name) => {
      return {
        sayHello: () => `Hello, I'm ${name}.`;
      }
    }

    // use normal function declaration to preserver `this`.
    const Human = function(name) {
      return {
        ...getName(name),
        ...canSayHello(name)
      }
    }

    const kakamotobi = Human("Kakamotobi"); // { name: "Kakamotobi", sayHello: () => "Hello, I'm Kakamotobi." }
    console.log(kakamotobi.sayHello()); // Hello, I'm Kakamotobi.
    ```
    - The mixin pattern can be powerful but in its current form we lose all of the ergonomics of Class-based OOP.
      - TypeScript allows us to use mixins in a Class-based format.
      - Instead of encapsulating everything in a single Class, create behavior classes that define the individual behaviors.
    ```ts
    function applyMixins(//) {
      //
    }
    
    // Mixin 1
    class CanSayHello {
      name;
      
      sayHello() {
        return `Hello, I'm ${this.name}`;
      }
    }
    
    // Mixin 2
    class HasSuperPower {
      heroName;
      
      superpower() {
        return `${this.heroName} beams light off eyes!`;
      }
    }
    
    // Build up SuperHero using mixins 
    class SuperHero implements CanSayHi, HasSuperPower {
      heroName;
      
      constructor(public name) {
        this.heroName = `HERO ${name}`;
      }
      
      sayHello: () => string;
      superpower: () => string;
    }
    
    applyMixins(SuperHero, [CanSayHello, HasSuperPower]);
    
    const superman = new SuperHero("Super Man");
    console.log(superman.superpower()); // HERO Super Man
    ```
##### Polymorphism
> the presentation of one interface for multiple data types. | MDN

> In the case of OOP, by making the class responsible for its code as well as its own data, polymorphism can be achieved in that each class has its own function that (once called) behaves properly for any object. | MDN

- Polymorphism = many forms.
- A superclass acts as a single interface that works for all subclasses of any data type.
  - i.e. the practice of designing objects to share behaviors and to be able to override shared behaviors with specific ones.
  - Ex: integers and floats are polymorphic since they have the same behaviors (Ex: add, subtract) despite having being different types.
- Allows us to get rid of long if and elses or switch and case statements.
#### Example
- You do not have to specify that `Dog` needs to speak like a dog, and `Cat` needs to speak like a dog, and `Cow` needs to speak like a cow. Simply call the `.speak()` method and the dog or cat or cow will do that.
```ts
class Animal {
  constructor() {}

  speak() {
    console.log("This will be overriden by the method in a subclass.");
  }
}

class Dog extends Animal {
  // ...
  speak() {
    console.log("woof");
  }
}

class Cat extends Animal {
  // ...
  speak() {
    console.log("meow");
  }
}

class Cow extends Animal {
  // ...
  speak() {
    console.log("moo");
  }
}
```
### Reference
[Difference between Functional Programming and Object-oriented Programming - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-functional-programming-and-object-oriented-programming/)  
[Functional Programming VS Object Oriented Programming - Medium](https://medium.com/@shaistha24/functional-programming-vs-object-oriented-programming-oop-which-is-better-82172e53a526)  
[Master the JavaScript Interview: What is a Pure Function? | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)  
[Master the JavaScript Interview: What is Functional Programming? | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)  
[Object Oriented vs Functional Programming with TypeScript - YouTube](https://www.youtube.com/watch?v=fsVL_xrYO0w&ab_channel=Fireship)  
[MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary)  

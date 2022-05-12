# Design Patterns
> In software engineering, a software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. Rather, it is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system. | Wikipedia

- Design patterns are solutions to commonly occuring problems in software design (object-oriented systems).
- There are three main classifications: **Creational**, **Structural**, and **Behavioral**, *defined by the "Gang of Four"*.

## Table of Contents
- [Creational Design Patterns](#creational-design-patterns)
  - [Singleton](#singleton)
  - [Prototype](#prototype)
  - [Builder](#builder)
- [Structural Design Patterns](#structural-design-patterns)
- [Behavioral Design Patterns](#behavioral-design-patterns)
- [Architectural Design Patterns](#architectural-design-patterns)
  - [Model-View-Controller (MVC)](#model-view-controller-mvc)
- [Reference](#reference)

## Creational Design Patterns
> Creational Patterns provide object creation mechanisms that increase flexibility and reuse of existing code.

- i.e., how objects are created.
### Singleton
- **Singleton is a creational pattern that allows a class to be instantiated only once.**
- Therefore, the `new` keyword on a Singleton should only instantiate that class only once in the whole system.
- Singletons can be useful when just one object is needed to coordinate a particular action across the system.
  - i.e., when you need to create global objects across the application.
  - Ex: a database connection object
#### Singleton in JavaScript
- In JavaScript (ES6+), the concept of global data and object literals exist, where objects are passed around by reference.
- *The basic characteristics of Singletons can be achieved by simply creating a global object.*
  - Ex: `const globalObj = { name: "globalObj", description: "I am a singleton" }`
#### Example
```js
class MyClass {
  constructor() {
    // If the class has been instantiated already.
    if (MyClass.instance) {
      console.log("Singleton classes cannot be instantiated more than once.");
      console.log("This is the previously instantiated Singleton.");
      return MyClass.instance;
    }
    MyClass.instance = this;

    // Other constructor code.
  }

  // Properties and methods.
}

const myClass1 = new MyClass();
const myClass2 = new MyClass();

console.log(myClass1 === myClass2); // true

export default myClass1;
```
### Prototype
- **Prototype is a creational pattern that lets you clone existing objects.**
- An alternative way to implement inheritance. Instead of inheriting from a class (subclassing), it comes from an object that has already been created.
  - Class inheritance leads to complicated hierarchy.
  - Inheriting via prototype creates a flat prototype chain, making it easier to share functionality between objects.
#### Prototype in JavaScript
- JavaScript supports prototypal inheritance.
#### Example
```js
const human = {
  greetings() {
    return "Hi. I am a human.";
  }
}

const tom = Object.create(human, { name: { value: "tom" }});

tom; // { name: "tom" }
Object.getPrototypeOf(tom); // { greetings: f }
tom.greetings(); // Hi. I am a human."
```
### Builder
- **Create objects step by step using methods rather than using the constructor.**
- Using the same construction code, different types and representations of an object can be made.
- Using the builder design pattern, there is no need for subclasses. 
#### Builder in JavaScript
- This pattern is encountered frequently in libraries like jQuery.
- However, it is going out of style in recent years.
#### Example
```js
class Omelette {
  constructor() {}
  
  addTomato() {
    this.tomato = true;
    return this;
  }
  addOnion() {
    this.onion = true;
    return this;
  }
  addHam() {
    this.ham = true;
    return this;
  }
}

const myOmelette = new Omelette();
const yourOmelette = new Omelette();

myOmelette
  .addTomato()
  .addOnion();

yourOmelette
  .addHam();
```

## Structural Design Patterns
> Structural Patterns explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.

- i.e., how objects relate to each other.

## Behavioral Design Patterns
> Behavioral Patterns take care of effective communication and the assignment of responsibilities between objects.

- i.e., how objects communicate with each other.

## Architectural Design Patterns
- These are fundamental structural organizations for software systems.
- More higher-level properties and mechanisms of a system, and separation of concerns.
### Model-View-Controller (MVC)
> MVC is an architectural design pattern that encourages improved application organization through a separation of concerns. It enforces the isolation of business data (Models) from user interfaces (Views), with a third component (Controllers) traditionally managing logic and user-input. | patterns.dev
- MVC is an architectural pattern that encompasses several design patterns defined by the "Gang of Four".
  - Ex: Observer pattern, etc.
  - The design patterns used may depend on the particular implementation.
- MVC is a design pattern that we as developers can adopt/apply.
  - Angular.js is an MVC framework.
  - React is not an MVC framework. It is a View library. But we can use the MVC pattern for a React component.
### Brief History
- Smalltalk-80 MVC was designed in 1979.
- It aimed to separate out the application logic from the UI.
  - Idea: decoupling the application logic and the UI would allow the reuse of models for other interfaces in the application.
- MVC
  - A Model represented domain-specific data that was separate from the UI (Views and Controllers).
  - A View represented the current state of a Model. Whenever changes were made to the Model, the Observer pattern was used to inform the View.
    - A View-Controller pair was required for each element displayed on the screen.
      - The View took care of presentation.
      - The Controller handled user interactions (Ex: clicks, key-presses).
- Key point: the View observes the Model. So when the Model changes, the View react.
### JavaScript MVC
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/MVC.png" alt="MVC Illustration" width="80%" /><br>
  <em>The definition and role of each can differ based on the project/implementation.</em>
</p>

#### Models
- **Manage the data/state for an application.**
- They are not concerned with the UI or presentation layers.
- They simply represent data and logic related to data that an application may require.
- When the Model changes (Ex: updated data), its observers (views) are notified of it.
- Model persistence is achieved by saving its most recent state in memory, localStorage, or synchronized with a database.
- A Model may have multiple views observing it.
#### Views
- **Visual representations of models that present a filtered view of their current state.**
- Concerned with building and maintaining a DOM element.
- A View observes its Model and updates itself accordingly whenever the Model changes.
- Views are simply "dumb" user interface that provides user interaction (Ex: get, set values in Models).
  - They are not responsible for any actions.
  - *Controllers are the ones responsible for actually updating Models.*
#### Controllers
- **The intermediary between Models and Views that are responsible for updating the Model upon user interaction/manipulation of the View.**
- Handles business logic and events (usually triggered by the user).


## Reference
[Software design pattern - Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern)  
[Design Patterns - Refactoring Guru](https://refactoring.guru/design-patterns/classification)  
[10 Design Patterns Explained in 10 Minutes - YouTube](https://www.youtube.com/watch?v=tv-_1er1mWI&ab_channel=Fireship)  

[Learn JavaScript Design Patterns - patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/)  
[Elements of MVC in React. Let’s discover the original MVC pattern… | by Daniel Dughila | The Startup | Medium](https://medium.com/swlh/elements-of-mvc-in-react-9382de427c09#:~:text=React%20isn't%20an%20MVC,nothing%20to%20do%20with%20frameworks.)  
[How JavaScript works: modularity and reusability with MVC | by Ukpaiugochi | SessionStack Blog](https://blog.sessionstack.com/how-javascript-works-writing-modular-and-reusable-code-with-mvc-16c65cbd9f64)  

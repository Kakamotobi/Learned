# Design Patterns
> In software engineering, a software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. Rather, it is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system. | Wikipedia

- Design patterns are solutions to specific technical problems or commonly occuring problems in software design (object-oriented systems).
- There are three main classifications: **Creational**, **Structural**, and **Behavioral**, *defined by the "Gang of Four"*.

## Table of Contents
- [Creational Design Patterns](#creational-design-patterns)
  - [Singleton](#singleton)
  - [Prototype](#prototype)
  - [Builder](#builder)
  - [Factory](#factory)
- [Structural Design Patterns](#structural-design-patterns)
  - [Facade](#facade)
  - [Proxy](#proxy)
- [Behavioral Design Patterns](#behavioral-design-patterns)
  - [Iterator](#iterator)
  - [Observer](#observer)
  - [Mediator](#mediator)
  - [State](#state)
- [Architectural Design Patterns](#architectural-design-patterns)
  - [Model-View-Controller (MVC)](#model-view-controller-mvc)
- [Reference](#reference)

## Creational Design Patterns
> Creational Patterns provide object creation mechanisms that increase flexibility and reuse of existing code.

- i.e., how objects are created.
### Singleton
- **Singleton is a creational pattern that allows a class to be instantiated only once.**
- Therefore, the `new` keyword on a Singleton should instantiate that class only once in the whole system.
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
- JavaScript objects have an internal property: prototype.
  - It is a reference to another object that contains properties and methods shared by all instances of the object.
  - When a constructor function is invoked, the new instance inherits the properties and methods of the constructor function.
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
### Factory
- **Create objects using an interface (function or method) without exposing the creation logic.**
  - Instead of using the `new` keyword to instantiate an object, use a function or method to instantiate it.
- Instead of conditionally creating objects on the get go, create a subclass or function (factory) that will determine which object to instantiate.
- Use a static factory method that creates and returns instances while hiding the details from the client.
- *Focused on hiding the details of constructing instances.*
#### Example
```js
class MacButton {}
class WindowsButton {}

// Without Factory (not very maintainable)
const button1 = os === "mac" ? new MacButton() : new WindowsButton();
const button2 = os === "mac" ? new MacButton() : new WindowsButton();

// With Factory
class ButtonFactory {
  static createButton(os) {
    return os === "mac" ? new MacButton() : new WindowsButton();
  }
}

const buttonFactory = new ButtonFactory();
const button1 = buttonFactory.createButton("mac");
const button2 = buttonFactory.createButton("windows");
```

## Structural Design Patterns
> Structural Patterns explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.

- i.e., how objects relate to each other.
### Facade
- **Provide a simplified interface to a library, framework, or any other complex set of classes, effectively hiding other low-level details in the codebase that the user does not need to know about.**
- *Focused on hiding implementations.*
#### Facade in JavaScript
- Almost every package installed with JavaScript can be considered a facade in some way.
  - Ex: jQuery acts as a facade for more low-level JS features.
#### Example
```js
// Low-level complexities.
class PlumbingSystem {
  setPressure(num) {}
  turnOn() { console.log("plumbing turned on!") }
  turnOff() { console.log("plumbing turned off!") }
}
class ElectricalSystem {
  setVoltage(num) {}
  turnOn() { console.log("electricity turned on!") }
  turnOff() { console.log("electricity turned off!") }
}

// Facade class containing the low-level systems as dependencies.
class House {
  #plumbing = new PlumbingSystem();
  #electricity = new ElectricalSystem();
  
  turnOnSystems() {
    this.#plumbing.setPressure(500);
    this.#plumbing.turnOn();
    this.#electricity.setVoltage(120);
    this.#electricity.turnOn();
  }
  
  turnOffSystems() {
    this.#plumbing.turnOff();
    this.#electricity.turnOff();
  }
}

const myHouse = new House();
myHouse.turnOnSystems();
myHouse.turnOffSystems();
```
### Proxy
- **Provide a substitute (proxy) for another object.**
- It controls and manages access to the target object.
  - Therefore, you can execute something before or after the request gets through to the target object.
- Analogy: credit cards are a proxy for money in the bank account. Credit cards can be used in place of cash and provides access to that cash.
#### Types of Proxies
- **Virtual Proxy**
  - A placeholder for objects that are expensive to create. The real object is created when it is first needed.
  - Used to perserve memory from being allotted to an object that may not be used in the future.
  - For example, lazy initialization can be implemented using virtual proxy.
    - Lazy initialization: instead of always executing exhaustive processes (creating the object on app launch), execute the process (initialize the object) only when it is needed.
- **Protection Proxy**
  - Used to control client's access to the object.
  - For example, the proxy can check if the client is authorized to access the object before passing the request to the object.
- **Remote Proxy**
  - Used to represent the object that is located on a remote server.
  - Communicating with a remotely located object may require complexities of working with the network. This is taken care of by a remote proxy as it passes the client request over the network.
- **Logging Proxy**
  - Used to keep the list of requests sent before passing it on to the object.
- **Caching Proxy**
  - Used to cache the results of recurring requests that always has the same results, and manage the life cycle of the cache.
- **Smart Reference**
  - Used to dismiss exhaustive objects once no clients need it any more.
  - For example, keep track of clients that are actively using the object. If there are no clients using it, dismiss the object.
#### Example - caching proxy
- There is a music downloading class where multiple requests from the client for the same music results to repetitive downloads.
- Instead of doing that, use a proxy class with the same interface as the original music downloading class, and cache files and return the cache for henceforth requests for the same music.
#### Example - [Vue Reactivity System](https://vuejs.org/guide/extras/reactivity-in-depth.html)
- The UI needs to be updated whenever the data changes.
- Vue does this by replacing the original object with a proxy.
```js
const originalObject = { name: "kakamotobi" };

const reactive = new Proxy(originalObject, {
  get(target, key) {
    console.log(`Tracking: ${key}`);
    return target[key];
  },
  set(target, key, value) {
    console.log("Updating UI"); // Tell the framework to rerender.
    return Reflect.set(target, key, value); // Use Reflect to update the data on the original object.
  }
});

reactive.name; // "Tracking: name" // "kakamotobi"

reactive.name = "bomboclat"; // "Updating UI" // "bomboclat"

reactive.name; // "Tracking: name" // "bomboclat"
```

## Behavioral Design Patterns
> Behavioral Patterns take care of effective communication and the assignment of responsibilities between objects.

- i.e., how objects communicate with each other.
### Iterator
- **Traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc).**
- Extract the traversal behavior of a collection into a separate object called an *iterator*.
  - Ex: `depthIterator`, `breadthIterator` objects to traverse a `treeCollection`.
  - Since each iterator object is separate, multiple iterators can traverse the same collection at the same time.
- All iterators should share the same interface, allowing compatibility with any type of collection or algorithm. If a special approach is needed, simply create a new iterator class instead of changing the collection.
- Analogy: free roaming, virtual guide app, local tour guide, are all different ways (iterators) to tour an area (collection).
### Observer
- **Define a subscription mechanism where observers are notified and updates in accordance when the object that they are observing changes.**
- One to many dependency between objects.
  - Observers depend on an object to provide them data.
- The main object should:
  - manage its main state.
  - implement a subscription mechanism that allows other objects to subscribe or unsubscribe to it.
  - keep track of its observers.
  - implement a notifying method that calls for updates in its observers.
    - Pass down the context of the changed data to its observers.
    - Ex: `for (let s of subscribers) s.update(this);`
  - other business logic.
- Observers perform certain actions upon receiving a notification from the main object.
  - All observers should implement the same interface to communicate with the main object.
- Ex: the MVC architecture (when the model changes, the view updates).
- Ex: rxjs library.
  ```js
  import { Subject } from "rxjs";
  
  const magazine = new Subject(); // Data to listen to.
  
  // The subject keeps track of these subscriptions and call their respective callback functions whenever the data changes.
  const subscriber1 = magazine.subscribe(v => console.log(`subscriber1: ${v}`));
  const subscriber2 = magazine.subscribe(v => console.log(`subscriber2: ${v}));
  
  // Push new values to the subject.
  // For each change in data, all subscribers will be notified.
  magzine.next("Can too much vegetables be bad for you?");
  magazine.next("Plant-based meat are not really plant-based.");
  magazine.next("The dangers of misinformation");
  ```
### Mediator
- **Define a mediator object that is interposed between objects that depend on each other, effectively organizing chaotic dependency relationships.**
- The objects cannot directly communicate with each other and the mediator is the only way for the objects to communicate.
- Instead of many-to-many, implement full object status.
- Analogy: traffic light, air traffic control tower.
- The use of mediators provides a separation of concerns and eliminates code duplication.
  - For example in a form, instead of having the logic of form elements depending on each other directly in the form elements, a mediator can be used.
    - A checkbox element, when triggered, does not communicate with the element that is is intended to open. It simply informs the mediator about the event along with any necessary context, which then the mediator will delegate to the target element.
  - This effectively allows easier code reuse in other parts of the application.
#### Example
- Middlewares in Express.js interpose between incoming requests and outgoing responses.
```js
import express from "express";
const app = express();

const logger = (req, res, next) => {
  console.log(`Request Type: ${req.method}`);
  next();
}

app.use(logger);

app.get("/", (req, res) => {
  res.send("Here is your response");
});
```
### State
- **Define an object that behaves differently when its internal state changes.**
- The idea is related to finite-state machine.
  - Finite-state machine: at any given time, the machine can be in exactly one of a finite number of states. The state can change in response to inputs.
- "State" is simply a set of values of the object's fields.
- Create new classes for every possible states of the object and move all behaviors related to state into those classes.
  - Change the active state object to another object representing the new state.
  - This requires all state classes to have the same interface.
    - Same method but different outcomes.
- The states may be aware of each other, and can initiate transitions.
- Analogy: buttons on a phone execute different functions depending on whether the phone is locked or not.

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

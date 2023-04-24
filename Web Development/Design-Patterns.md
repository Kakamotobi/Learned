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
  - [Pub/Sub](#pubsub)
  - [Mediator](#mediator)
  - [State](#state)
- [Architectural Design Patterns](#architectural-design-patterns)
  - [What is meant by Architecture?](#what-is-meant-by-architecture)
  - [MV<sup>*</sup> Architectures](#mv-architectures)
    - [Evolution of MV<sup>*</sup> Architectures](#evolution-of-mv-architectures)
      - [Introduction of Model-View-Controller (MVC)](#introduction-of-model-view-controller-mvc)
      - [Early Web MVC](#early-web-mvc)
      - [jQuery Era MVC (MVP)](#jquery-era-mvc-mvp)
      - [The Rise of Model-View-ViewModel (MVVM) and SPA](#the-rise-of-model-view-viewmodel-mvvm-and-spa)
      - [Container and Presentational Components](#container-and-presentational-components)
      - [The Flux Pattern and Redux](#the-flux-pattern-and-redux)
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
- **Define a subscription mechanism where observers are notified (typically by calling a method on each observer) and updates in accordance when the object that they are observing changes.**
  - _The observable maintains a list of its observers._
  - _However, observers are not "aware" of the observable._
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
### Pub/Sub
- **Define a subscription mechanism where subscribers are notified through a topic/channel that the publisher publishes to.**
  - _i.e. a publisher publishes to the topic/channel, and subscribers subscribe to that topic/channel._
  - _Publishers and subscribers are unaware of each other._
- Often used in distributed systems and message-oriented middleware.
#### Example
```js
const subscriber = function(data) {
    console.log('received event');
};

PubSub.on("some-event", subscriber); // register the subscriber on the `PubSub` object on the `"some-event"` channel.

PubSub.publish("some-event", "data"); // the publisher publishes `"some-event"` through the `PubSub` object.
```
### Mediator
- **Define a mediator object that is interposed between objects that depend on each other, effectively organizing chaotic dependency relationships.**
  - _i.e. a central hub that facilitates communication between different objects._
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
### What is meant by Architecture?
- Architecture is a generic term that applies to a broad range of levels.
- Architecture is the ***shared understanding*** of the system design of a project between the developers familiar with the codebase.
  > This understanding includes how the system is divided into components and how the components interact through interfaces. These components are usually composed of smaller components, but the architecture only includes the components and interfaces that are understood by all the developers. | Martin Fowler
  - It is important to have a good shared understanding between the developers of the project, especially as the project grows.
- Architecture is ***decisions that are hard to change***.
- Architecture provides the means for us to structure our application in a consistent and sustainable manner.
#### How to Design an Architecture
> Whether something is part of the architecture is entirely based on whether the developers think is important. | Martin Fowler
- When deciding how to design the architecture of a project, the first thing to do is to figure out what is important.
- Ask theses questions:
  - What are the key things about this project/system?
  - What is the most important thing in the codebase that we have to keep at the top of our heads when working on it?
#### Why is it Important?
- In the short-term, any code that works will suffice, which means quick launching and profits.
  - However, as the project/service grows, it will become more difficult and take longer to add new features and maintain the service.
- With good architecture and effort in internal quality, it will be much easier to add new features, migrate, and maintain the service.
  - In the long-term, good architecture is crucial and provides economic value.
### MV<sup>*</sup> Architectures
- MV<sup>*</sup> architectures are common architectural design patterns that have become popular with the rise of single-page applications.
- They are focused on separation of concerns to decouple components from each other, allowing for more independent, reusable, scaled components.
- Three popular MV* variants are MVC, MVP, and MVVM.
  - They differ from each other in terms of type and level of coupling between the components.
- They can encompass several design patterns defined by the "Gang of Four".
  - Ex: observer pattern, etc.
  - The design patterns used may depend on the particular implementation.
- They are patterns that we as developers can adopt/apply.
  - *React is not an MVC framework. It is a View library.*
- ***Don't get too hung up on the names. Different frameworks have different naming conventions.***
#### Evolution of MV<sup>*</sup> Architectures
##### Introduction of Model-View-Controller (MVC)
- Smalltalk-80 MVC was designed in 1979.
- It aimed to separate out the application logic from the UI.
  - Idea: *decoupling the application/business logic and UI would allow the reuse of models for other interfaces in the application.*

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/original-MVC.png" alt="Original MVC Illustration" width="80%" />
</p>

- **Model**
  - Represents domain-specific data that was separate from the UI (Views and Controllers).
  - It could be a single object or some structure of objects.
- **View**
  - A visual representation of the Model.
  - It took care of presentation.
  - It observes the Model. Therefore, whenever changes are made to the Model, the View changes accordingly.
  - It may also update the Model by sending appropriate messages.
- **Controller**
  - The link between a user and the system.
  - It handles logic and user inputs (Ex: clicks, keypresses).
  - It provides the user with relevant views for user input.
  - It receives the user's output and relays it to the View.
###### Problems
1) Both Views and Controllers shared responsibility for updating Models.
    - This is hard to maintain. Therefore, a unidirectional data flow is preferrable.
    - Ex: View &rarr; Action &rarr; State &rarr; View
2) Loose coupling and high cohesion
    - Views are aware of Models. Controllers are aware of both Views and Models.
      - Models are dependencies for Views and Controllers.
    - Hence, changing the Model interface means both the Views and Controllers have to be changed accordingly.
    - This is not ideal. It is preferrable to minimize the number of dependencies, effectively separating concerns.
      - Ex: only allow the controller to manipulate the Model.
      - Therefore, the Controller's responsibility began evolving from handling user input to being a mediator between the Model and the View.
##### Early Web MVC
- Back when every page had to be built and sent from the server (MPA), MVC was mostly implemented on the server-side.
- The client would request updates via forms or links that the server would respond to with the updated page.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/early-web-MVC.png" alt="Early Web MVC Illustration" width="80%" />
</p>

- **Model**
  - The database.
- **View**
  - The client-side (HTML, CSS, JS).
- **Controller**
  - The back-end (handle data through server routers and create HTML to present).
  - Requests usually meant a whole new page had to be built from the server.
##### jQuery Era MVC (MVP)
- With the introduction of AJAX and increased role of the frontend, there became no need to build the page in the server.
- At this point, the focus was on minimizing dependency between the Model and the View, hence, HTML and jQuery were managed separately.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/jquery-era-MVC.png" alt="jQuery Era MVC Illustration" width="80%" />
</p>

- **Model**
  - **Represents the data (received through AJAX).**
- **View**
  - **Defines what is displayed to the user (presentation).**
  - Forwards user input to the Controller.
- **Controller**
  - **JS (handles data fetching and sending to the server through event listeners)**.
    - What was previously done by server routers is now done by JS.
  - jQuery's main functions were DOM traversal/manipulation, event handling, and AJAX.
  - Manipulates the Model by communicating with the server.
##### The Rise of Model-View-ViewModel (MVVM) and SPA
- Back when pages were built on the server, pages could be developed declaratively by embedding expressions (Ex: `{{}}`, `<%= %>`).
- However, for jQuery, data had to be handled manually (find, change, update, attach events, etc.).
- Therefore, like how pages were built declaratively from the server, developers searched for a way to build from the client declaratively, such as using templates.
- This led to the introduction of template binding based libraries that make use of declarative data bindings, such as knockout.js, and AngularJS.
  - In 2013, AngularJS revolutionized the paradigm of web development as it introduced templates and binding.
    - *The Controller's role remains the same but instead of manually manipulating the DOM using jQuery, things were done declaratively through these frameworks based on templates and binding.*
    - There was no longer the need to manipulate the DOM, as it was taken care of by the framework.
      - The "C" in MVC was replaced by "VM" simply to indicate that you only need to handle the Model to update the View.
- Instead of having multiple pages, it became possible to simply update parts of the page.
- Other frameworks and libraries of MVVM architecture (_or similar_) began to be released: Knockout.js, Vue, Angular, Svelte, React, etc.
  - _React Notes_
    - React is not opinionated to a particular architecture.
    - React is a _View_ library. Hence, React components represent the _View_ layer.
    - The responsibilities of the _ViewModel_ layer is essentially handled internally within the component itself since changes in the state triggers a re-render of the component.
    - To implement a structure similar to a complete MVVM, React can be used with other libraries such as Redux. Redux becomes the _Model_ layer, as it manages the states of the application.
      - Therefore, a very simple React application without using any complex React APIs or other libraries could be seen as "VVM" where V represents JSX and CSS, and VM represents component-related code for managing the state.
  - _Svelte Notes_
    - Svelte is similar to the MVVM architecture.
    - However, Svelte does not have a separate concept of a ViewModel. Instead, it provides a built-in reactivity system that allows us to write reactive code directly in components.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/MVVM.png" alt="MVVM Illustration" width="80%" />
</p>

- **Model**
  - **Represents domain-specific data and business logic.**
- **View**
  - **Defines what is displayed to the user (presentation).**
  - Forwards user input to the ViewModel via the data binding linking the two.
    - The View directly binds to properties on the ViewModel to send and receive updates.
    - i.e. it observes the ViewModel.
  - It represents how the ViewModel's functionality is displayed.
- **ViewModel**
  - **Defines properties and logic for the View to data bind to, and informs the View of changes to state (presentation logic).**
    - Communication with the View is automated through binding.
  - i.e. it contains the Model used by the View, and changes made to the View are reflected to the ViewModel, and vice versa.
  - Model that draws the View.
  - It defines the structure and behavior of the application.
  - It represents the functionality offered by the UI.
###### Transition from MVC to MVVM
- **Issues at Point**
  - Unlike before, there are a lot of Views in modern Frontend.
  - Data needs to go both ways (between Views and Models).
    - Handling this in a Controller leads to a huge, complex Controller.
  - Views need to be organized in a hierarchical structure in order to optimze re-renders.
- **Controller vs ViewModel**
  - The term "Controller" makes more sense in backend development or in server rendered applications where it facilitated the entire application.
    - Ex: when websites were generated in the backend and wholy sent as a response.
  - The "ViewModel" is only responsible for syncing the current state with the data in the Model, and data binding with the View.
- Model and View are not separated, and rather managed using a template.
  - Previously, HTML was accessed through the DOM (using selectors like classes and ids).
  - Now, HTML can be directly accessed (Ex: JSX).
##### Container and Presentational Components
- a.k.a. Smart and Dumb, Stateful and Pure, etc.
- Container components handle the business logic.
  - i.e. concerned with how things work.
  - Act as data sources to other components, hence are often stateful.
- Presentational components simply distribute data.
  - i.e. concerned with how things look.
  - They are rarely stateful.
    - Any state that exists is usually for UI purposes rather than data.
  - Have no dependencies on the rest of the application.
###### The Problem
- By emphasizing and enforcing component reusability and independence, sharing of data between components led to **props drilling**.
  - Props drilling is when a component passes on state as props deep into its descendants.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/props-drilling.gif" alt="Props Drilling" width="80%" /><br>
</p>

- It's difficult to track, and components in the middle that do not need to know about the state has to hold on to it.
- To tackle this, a new architectural pattern for state management emerged.
##### The Flux Pattern and Redux
- Modern frontend applications have a lot of Views, and data needs to go both ways between Views and Models. This can lead to complicated dependencies and inconsistencies.
<p align="center">
  <img src="https://www.freecodecamp.org/news/content/images/2020/04/Screenshot-2020-04-16-at-6.38.14-PM.png" alt="MVC Problem" width="40%" />
</p>

- Flux was developed by Facebook as a way to solve this and manage state in large-scale, complex web applications.
- **Flux is a pattern that adopts a _uni-directional data flow_ (View &rarr; Store &rarr; View) in order to ensure predictable and consistent state, and solve scaling issues, such as props drilling, while using the MVC approach on the client-side.**
- Instead of perceiving the View as each MVC component, the components are perceived as a whole View.
- Many state management libraries such as Redux use the Flux architecture.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/state-management.gif" alt="State Management" width="80%" />
</p>

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/flux.png" alt="Flux Pattern" width="80%" />
</p>

- The View, using the Dispatcher, makes a call to the Action. The Action, using a Reducer, stores the data in Store. The data in Store then updates the View.
- **Actions**
  - **The methods provided by the Dispatcher to trigger a dispatch to the Stores and most times comes with a payload of data.**
  - Actions can also come from the server (Ex: during data initialization, when the server returns an error, etc.).
- **Dispatcher**
  - **An object that broadcasts Actions to the Store, to update the state.**
  - All data flows through the dispatcher as a central hub.
  - It is basically a registry of callbacks into the stores.
- **Store**
  - **Manages the application state (both domain state and UI state) and logic.**
  - Has a similar role to a Model in MVC.
    - The difference is that Store manages the state of multiple objects.
- **Views and Controller-Views**
  - **Controller-Views listen for events that are broadcast by the Stores that they depend on.**
    - They provide the code to get the data from the Stores and pass the data down the chain of its descendants.
    - Upon receiving the event from the Store, it requests the new data it needs via the Stores' public getter methods. It then calls its own `setState()` method causing its `render()` method and the `render()` method of all its descendants to run.
###### Transition from MVC to Flux
- As State Management was introduced, the commonly shared business logic layer and the View layer were completely separated.
- A single component is not considered the View anymore. All components combined as a whole is considered the View.
- The View and business logic are intermediated with the Action and Reduce interfaces. And the Controller is now one-way instead of two-way.
###### Problems with the Flux Pattern
- High learning curve.
- Complicated syntax.

## Reference
### Creational, Structural, Behavioral Design Patterns
[Software design pattern - Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern)  
[Design Patterns - Refactoring Guru](https://refactoring.guru/design-patterns/classification)  
[10 Design Patterns Explained in 10 Minutes - YouTube](https://www.youtube.com/watch?v=tv-_1er1mWI&ab_channel=Fireship)  
### Architectural Design Patterns
[Making Architecture Matter - Martin Fowler Keynote - YouTube](https://www.youtube.com/watch?v=DngAZyWMGR0&ab_channel=O%27Reilly)  
[Who Needs an Architect? | Martin Fowler](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf)  
[Learn JavaScript Design Patterns - patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/)  
[프론트엔드에서 MV* 아케틱쳐란 무엇인가요?](https://velog.io/@teo/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%EC%97%90%EC%84%9C-MV-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94)  
[Do MVC like it’s 1979. How to build a good iOS architecture by… | by Bohdan Orlov | Bumble Tech | Medium](https://medium.com/bumble-tech/do-mvc-like-its-1979-da62304f6568)  
[Elements of MVC in React. Let’s discover the original MVC pattern… | by Daniel Dughila | The Startup | Medium](https://medium.com/swlh/elements-of-mvc-in-react-9382de427c09#:~:text=React%20isn't%20an%20MVC,nothing%20to%20do%20with%20frameworks.)  
[javascript - MVVM architectural pattern for a ReactJS application - Stack Overflow](https://stackoverflow.com/questions/51506440/mvvm-architectural-pattern-for-a-reactjs-application)  
[Presentational and Container Components | by Dan Abramov | Medium](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)  
[In-Depth Overview | Flux](https://facebook.github.io/flux/docs/in-depth-overview/)  
[react-redux.md](https://gist.github.com/rogerwschmidt/8739668f73edc79e1581b63266719257)  

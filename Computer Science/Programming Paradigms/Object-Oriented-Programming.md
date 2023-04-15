## Object-Oriented Programming (OOP)

## Table of Contents
- [What is Object-Oriented Design(OOD)](#what-is-object-oriented-designood)
  - [SOLID](#solid)
    - [Single-responsibility Principle](#single-responsibility-principle)
    - [Open-closed Principle](#open-closed-principle)
    - [Liskov substitution Principle](#liskov-substitution-principle)
    - [Interface segregation Principle](#interface-segregation-principle)
    - [Dependency inversion Principle](#dependency-inversion-principle)
- [What is Object-Oriented Programming(OOP)?](#what-is-object-oriented-programmingoop)
- [Building Blocks of OOP](#building-blocks-of-oop)
- [The 4 Pillars of OOP](#the-4-pillars-of-oop)
  - [Encapsulation](#encapsulation)
  - [Abstraction](#abstraction)
  - [Inheritance](#inheritance)
  - [Polymorphism](#polymorphism)
- [Example](#example)
- [Object Instances and Reference](#object-instances-and-reference)
- [Some Tips on Implementing OOP](#some-tips-on-implementing-oop)

## What is Object-Oriented Design(OOD)?
> Object-oriented design (OOD) is the process of planning a system of interacting objects for the purpose of solving a software problem. | Wikipedia

- OOD is one of many approaches in designing a software system.
### SOLID
#### Single-responsibility Principle
> A class should have one and only one reason to change, meaning that a class should have only one job. | DigitalOcean

- **If another responsibility pops up, isolate it to another class to ensure that an individual class is only concerned with doing one thing.**
- Ex: a class that fetches data from a server, should only be doing that and not doing anything else like rendering the data.
- Ex: a shape class should only contain properties and methods pertaining to that shape, such as calculating the area, and should not contain logic to, for example, output to the console.
#### Open-closed Principle
> Objects or entities should be open for extension but closed for modification. | DigitalOcean

- i.e. **we should be able to extend a class without modifying the class itself.**
- Example
  - The `sum` method on the `AreaCalculator` class calculates the area of each shape that it was given and returns a sum of the areas.
  - This means that every time a new shape is added, the `AreaCalculator` class will have to be modified (need to add more `if`/`else`).
  - Instead, move the logic to calculate the `area` to the individual shapes' classes.
  - The `AreaCalculator` class' `sum` method should now only invoke the `area` method of the shape objects that it was given and return a sum of the areas.
  - Therefore, the `AreaCalculator` class does not have to be constantly modified. Just make sure that all shapes share the same interface to get its area.
#### Liskov substitution Principle
> Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T. | DigitalOcean

- i.e. subclasses or derived classes should be substitutable for their base or parent class.
- i.e. **when a subclass overrides its parent's property/method, it must be/return the same type.**
#### Interface segregation Principle
> A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use. | DigitalOcean

- Example
  - The `ShapeInterface` consists of two methods: `area()` and `volume()`.
  - 2D shapes do not need the `volume()` method, however, the `ShapeInterface` unnecessarily forces 2D shapes to also include that interface.
  - Therefore, create separate interfaces for 2D and 3D shapes.
#### Dependency inversion Principle
> Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions. | DigitalOcean

- **i.e. high level modules should not depend on low level modules. Rather, they should both depend on abstractions.**
  - Each class should not care of what exactly other classes are doing.
  - _To do this, we can setup an interface through which both classes can communicate without "knowing" each other._
- Abstraction vs Concretion
  - Abstraction: a high level description of something.
  - Concretion: a real instance of an abstraction.
- Dependency inversion principle allows for the decoupling of classes; thereby, leading to more testable and maintainable code.
- Example
  - The below code violates the dependency inversion principle because the high-level module `PasswordReminder` is forced to depend on an instance of the `MySQLConnection` class.
  - _If the DB is changed later on, `PasswordReminder` would have to be modified as well._
    ```js
    // The low-level module
    class MySQLConnection {
      connect() {
        // Connect to MySQL DB
        return "DB connected";
      }
    }
    
    // The high-level module
    class PasswordReminder {
      #dbConnection;
      
      constructor(dbConnection) { // dependency violation
        this.#dbConnection = dbConnection;
      }
    }
    ```
  - The below code complies to the dependency inversion principle since the high level module (`PasswordReminder`) does not depend on the low level module (`MySQLConnection`) but instead expect any DB connection class that complies with the `DBConnectionInterface`.
    ```ts
    interface DBConnectionInterface {
      connect(): string;
    }
    
    class MySQLConnection implements DBConnectionInterface {
      connect() {
        // Connect to MyQL DB
        return "DB connected";
      }
    }
    
    class PasswordReminder {      
      constructor(private dbConnection: DBConnectionInterface) { // set type to DBConnectionInterface
        this.dbConnection = dbConnection;
      }
    }
    ```

## What is Object-Oriented Programming(OOP)?
> Object-oriented programming (OOP) is a programming paradigm based on the concept of "objects", which can contain data and code. The data is in the form of fields (often known as attributes or properties), and the code is in the form of procedures (often known as methods). | Wikipedia

- **Structures a program into simple, reusable pieces of code (usually classes), which are used to create individual instances of objects.**
  - The properties on the objects are manipulated using the methods given to the object.
- It is an *imperative* type of programming style.
- OOP came to rise amidst ADT and Object-orientation advancing.
  - ADT focused on type abstraction.
    - It supports creating instances of a type.
  - Object-orientation focused on data.
    - It supports inheritance and polymorphism.
- The gist of OOP is to not do "do this for this type, do that for that type".

## Building Blocks of OOP
### Class
- A template/blueprint for creating objects.
- The common properties/methods of a class are loaded to heap memory.
### Instance
- The individual instantiation/realization of a class that is created at runtime.
- An instance is loaded to heap memory with only properties specific to it.
- Several instances together is called a collection.
### Object
- The term "object" in this case refers to the grouping of an instance and its class.
  - i.e. the pairing of the instance and the class.
  - Ex: if instanceA and instanceB are instances of classC, instanceA and classC are one object, and instanceB and classC are another.
### Property
- Properties are member variables and attributes describing the state of an object.
### Method
- Methods are member functions that represent the behaviors of an object.

## The 4 Pillars of OOP
### Encapsulation
> ... the packing of data and functions into one component (ex: a class) and then controlling access to that component to make a “blackbox” out of the object. | MDN

- OOP bundles/encapsulates relatable variables and methods that operate on them into an object.
- This reduces complexity and increases reusability.
### Abstraction
> ... a way to reduce complexity and allow efficient design and implementation in complex software systems. It hides the technical complexity of systems behind simpler APIs. | MDN

- OOP provides a simpler interface for the user since the complexity of a class is less exposed.
- This reduces the impact of change.
  - Any change within will not leak to the outside.
### Inheritance
> Data abstraction can be carried up several levels, that is, classes can have superclasses and subclasses. | MDN

- A child class extending from a base/parent class inherits all of the parent's functionality and is able to overwrite or extend it with custom behaviors it needs.
- This helps eliminate redundant code.
- _Note_
  - Be careful of deeply nested classes as it can become very difficult to debug (too much abstraction!).
#### Example
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
#### Composition
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
### Polymorphism
> the presentation of one interface for multiple data types. | MDN

> In the case of OOP, by making the class responsible for its code as well as its own data, polymorphism can be achieved in that each class has its own function that (once called) behaves properly for any object. | MDN

- Polymorphism = many forms.
- A superclass acts as a single interface that works for all subclasses of any data type.
  - i.e. the practice of designing objects to share behaviors and to be able to override shared behaviors with specific ones.
  - Ex: integers and floats are polymorphic since they have the same behaviors (Ex: add, subtract) despite having being different types.
- Allows us to get rid of long if and elses or switch and case statements.

## Example
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

## Object Instances and Reference
```js
const personA = new Person();
const personB = new Person();
```
### What's happening here?
- The shared properties/methods of class `Person` is loaded onto the heap memory.
- Two instances of `Person` are created. Hence, these instances (containing only properties that are specific to itself) is loaded onto the heap memory. When needed, the instances refer to their prototype (the class) to invoke inherited behaviors.
- This "grouping" of an instance and its class is an object.
- The variables `personA` and `personB` are pointers to those objects.
### Explained in Terms of Memory
- Pointer variables are loaded onto the stack.
- Instances (and Classes) are loaded onto the heap memory.
- Ex: the pointer variable personA is loaded onto memory. It points to the personA instance on heap memory, which references its class Person that is also loaded onto heap memory.

## Some Tips on Implementing OOP
- All methods relating to private members should be on the same class.
  - Resist from passing on an object's variable to another object to use.
  - Instead, keep an object's variables private and define methods for those variables in that object.
  - i.e. the higher object should not bring a lower object's values to process. The higher object should try to invoke the lower object's methods.
  - Ex: if a ChessPiece object has a Position object pertaining to it, the ChessPiece object should invoke functionalities defined in the Position object instead of bringing the position value and doing the calculation itself.
- The lower the object, the more likely that it should be more independent.

## Reference
[Object-oriented design - Wikipedia](https://en.wikipedia.org/wiki/Object-oriented_design)  
[SOLID: The First 5 Principles of Object Oriented Design  | DigitalOcean](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)  
[Object-oriented programming - Wikipedia](https://en.wikipedia.org/wiki/Object-oriented_programming)  
[Object Oriented vs Functional Programming with TypeScript - YouTube](https://www.youtube.com/watch?v=fsVL_xrYO0w&ab_channel=Fireship)  
[MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary)  

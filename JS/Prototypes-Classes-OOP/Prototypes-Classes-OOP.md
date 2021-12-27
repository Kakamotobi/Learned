# Prototypes, Classes, and OOP

## Table of Contents
- [Object Oriented Programming (OOP)](#object-oriented-programming-oop)
- [JavaScript and OOP](#javascript-and-oop)
  - [Object Prototypes](#object-prototypes)
- [Factory Function](#factory-function)
- [Constructor Function](#constructor-function)
- [Class](#class)
  - [Instance Methods vs. Class Methods](#instance-methods-vs-class-methods)
  - [`extends` and `super` Keywords](#extends-and-super-keywords)

## Object Oriented Programming (OOP)
> A programming paradigm that relies on the concept of classes and objects. It is used to structure a software program into simple, reusable pieces of code blueprints (usually called classes), which are used to create individual instances of objects.

> A class is an abstract blueprint used to create more specific, concrete objects. Classes often represent broad categories, like Car or Dog that share attributes. These classes define what attributes an instance of this type will have, like color, but not the value of those attributes for a specific object.

> Classes can also contain functions, called methods available only to objects of that type. These functions are defined within the class and perform some action helpful to that specific type of object.
### Reference
[What is Object Oriented Programming? OOP Explained in Depth](https://www.educative.io/blog/object-oriented-programming)

## JavaScript and OOP
- JavaScript is not a class-based object-oriented language.
- Rather, it is a *prototype-based language*.
  - This prototype object serves as a template for which features, methods, functionalities can be inherited.
### Object Prototypes
- The mechanism by which JS objects inherit features from one another.
- Objects can have a prototype object, which acts as a *template object* that it inherits properties and methods from.
- `.prototype` vs. `__proto__`
  - `.prototype` is the actual object where properties and methods are added to.
    - Ex: `Array.prototype` <-- prototype object that every array shares.
  - `__proto__` is a reference to the `.prototype` object.
    - It is a property name on a JS object.

## Factory Function
- Make a prototype object to which we can add properties and methods based off of arguments that have been provided, and then return that object.
- One way of making objects based off of a pattern/recipe (not ideal).
### Example
![factoryFunction](refImg/factoryFunction.png)
### Outcome
![facFuncTest](refImg/facFuncTest.png)
### Shortcoming
- Along with the unique properties, functions/methods are recreated and a unique copy is added to every object that is henceforth made.
- It is unnecessary and inefficient for each object to have its own copy of the function/method.
- i.e., the methods are not stored in `__proto__`.

## Constructor Function
- Another way of making objects based off of a pattern/recipe.
- Notes
  - Capital first letter indicates a constructor function.
  - There is never a `return` value in constructor functions.
  - `this` keyword is referenced directly in the function; not in an object.
- **`new`** Operator
  1. Creates a plain, blank JS object.
    - Ex: `const color = {};`
  3. Links (sets the constructor of) this object to another object.
  4. Passes the newly created object from Step 1 as the `this` context.
  5. Returns `this` object if the function doesn't return its own object.
    - Ex: `return color;`
### Example
![constructorFunction](refImg/constructorFunction.png)
### Outcome
![constrFuncTest](refImg/constrFuncTest.png)
### Shortcoming
- Things are not grouped together.
  - The constructor function and methods, are defined separately.

## Class
- **A blueprint for creating objects with pre-defined properties and methods.**
- JavaScript does not really have classes.
  - *`class` is just syntactic sugar over JavaScript's existing prototype-based inheritance.*
    - Syntactic sugar for what factory and construction functions do.
  - It does not introduce a new object-oriented inheritance model to JavaScript.
  - `class` is merely a dressed up function.
    - The below are the same.
      ```js
      const Pic = function(x,y,image) {
        this.x = x;
        this.y = y;
        this.image = image;
      }

      let dogPic = new Pic(300,300,"dog.jpg");
      ```
      ```js
      class Pic {
        constructor(x,y,image) {
          this.x = x;
          this.y = y;
          this.image = image;
        }
      }

      let dogPic = new Pic(300,300,"dog.jpg");
      ```
- Notes
  - Capital first letter indicates a class.
  - There is always a `constructor(){}` function, which executes immediately whenever a new class is created. 
  - Add properties and methods in one go.
  - Instances of classes are created with the `new` keyword.
### Instance Methods vs. Class Methods
#### Instance Methods
- Methods that provide functionality that pertains to an individual instance of a class.
- Example
  ```js
  class Student {
    constructor(firstName, lastName) {
      this.firstName = firstName;
      this.lastName = lastName;
    }

    fullName() {
      return `${this.firstname} ${this.lastName}`;
    }
  }

  let student1 = new Student("Daeram", "Chung");
  student1.fullName(); // "Daeram Chung"
  ```
#### Class Methods
- Using the `static` keyword, define methods or functionalities that is pertinent to classes but not necessarily to individual instances of a class.
- Static methods are called without instantiating their class and cannot be called through a class instance.
- Static methods are often used to create utility functions for an application.
- Example
  ```js
  class Student {
    constructor(firstName, lastName) {
      this.firstName = firstName;
      this.lastName = lastName;
    }
    
    static enrollStudents(...students) {
      // send an email, etc.
    }
  }
  
  let student1 = new Student("Daeram", "Chung");
  let student2 = new Student("Tom", "Jerry");
  
  Student.enrollStudents([student1, student2])
  ```
### Example
![class](refImg/class.png)
### Outcome
![classTest](refImg/classTest.png)

### `extends` and `super` Keywords
- Sharing functionality between classes, essentially subclassing/inheritance.
- **`extends`** Keyword
  - Allows the use of properties and methods from another (super) class.
  - The `constructor` and methods of the original class have priority to that of the super class.
- **`super()`** Keyword
  - References the super class that we are extending from.
  - It will call the `constructor` from the super class.
  - If the `constructor` needs other additional properties, instead of rewriting what's already written in the super class, do `super([arguments])` and only write the additional properties.
#### Example
![extendsAndSuper](refImg/extendsAndSuper.png)
#### Outcome
![spike](refImg/spike.png)

![tom](refImg/tom.png)

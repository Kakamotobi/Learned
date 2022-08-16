# Annotating Classes in TypeScript

## Table of Contents
- [Property Fields Type Annotation](#property-fields-type-annotation)
  - [Parameter Properties Shorthand](#parameter-properties-shorthand)
- [Class Fields Type Annotation](#class-fields-type-annotation)
- [The Public and Private Modifiers](#the-public-and-private-modifiers)
- [The Protected Modifier](#the-protected-modifier)
- [Getters and Setters Type Annotation](#getters-and-setters-type-annotation)
- [Classes and Interfaces (ft. `implements`)](#classes-and-interfaces-ft-implements)
- [Abstract Classes](#abstract-classes)

## Property Fields Type Annotation
- Type checking of properties of instances (ft. `readonly` modifier).
```js
class Player {
  readonly first: string; // For the property in the constructor (this.first)
  readonly last: string; // For the property in the constructor (this.last)
  
  constructor(first: string, last:string) {
    this.first = first;
    this.last = last;
  }
}

const tom = new Player("Tom", "Jerry");
tom.first = "Spike"; // Cannot assign to 'first' because it is a read-only property.
```
### Parameter Properties Shorthand
- An alternative to the above example.
- There is no need to initialize `this.` in the constructor.
```js
class Player {
  constructor(public readonly first: string, public readonly last: string) {}
}

const tom = new Player("Tom", "Jerry");
tom.first; // "Tom"
tom.last; // "Jerry"
```

## Class Fields Type Annotation
- Type checking `tom.score`.
```js
class Player {
  score: number = 0;
}

const tom = new Player("Tom", "Jerry");
tom.score = 3;
tom.score = "three"; // Type 'string' is not assignable to type 'number'.
```

## The Public and Private Modifiers
- By default, in JS and TS, every single property and methods in a class is considered public.
  - i.e. they can be accessed from any where.
  - Therefore, `public` is not necessary but can be used to be clear to other developers that it is used outside of the class.
- Private properties means that the property is only accessible in that instance of the specific class.
  - It is not accessible by child classes.
```js
class Player {
  public readonly first: string; // For the property in the constructor (this.first)
  public readonly last: string; // For the property in the constructor (this.last)
  private score: number = 0;
  // OR
  // #score: number = 0; // JavaScript private when targeting ES2015 or higher.
  
  constructor(first: string, last:string) {
    this.first = first;
    this.last = last;
  }
  
  private secretMethod(): void {
    console.log("secret method");
  }
}

const tom = new Player("Tom", "Jerry");
tom.score; // Property 'score' is private and only accessible within class 'Player'.
tom.secretMethod(); // Property 'secretMethod' is private and only accessible within class 'Player'.
```

## The Protected Modifier
- The protected modifier is used when working with inheritence.
- Protected properties means that the property cannot be accessible from outside the class except for child classes.
  - It is not accessible by components extending from that class.
```js
class Player {
  constructor (
    public first: string,
    public last: string,
    protected _score: number
  ) {}
}
```
```js
class SuperPlayer extends Player {
  public isAdmin: boolean = true;

  maxScore() {
    this._score = 99;
  }
}
```

## Getters and Setters Type Annotation
```js
class Player {  
  constructor(
    public first: string,
    public last: string
    private _score: number
  ) {}
  
  get fullName(): string {
    return `${this.first} ${this.last}`;
  }
  
  get score(): number {
    return this._score;
  }
  
  set score(newScore: number) {
    if (newScore < 0) return new Error("Scores cannot be negative");
    this._score = newScore;
  }
}

const tom = new Player("Tom", "Jerry", 3);
tom.fullName; // "Tom Jerry"
tom.score; // 3
tom.score = 2;
tom.score; // 2
```

## Classes and Interfaces (ft. `implements`)
- Interfaces can also describe the shape of a class.
```js
interface Colorful {
  color: string;
}

interface Printable {
  print(): void;
}

class Bike implements Colorful {
  constructor(public color: string) {}
}

class Jacket implements Colorful, Printable {
  constructor(public brand: string, public color: string) {}
  
  print() {
    console.log(`${this.color} ${this.brand} jacket`);
  }
}

const bike1 = new Bike("red");

const jacket1 = new Jacket("Prada", "black");
````

## Abstract Classes
- `abstract` means that you can not create a new instance of the class any more.
- Define pattern, methods that must exist in / be implemented by subclasses.
- Kind of like an interface.
  - The key difference is that abstract classes can have actually functioning methods.
    - Ex: interfaces cannot have the `greet` function in the `Employee` class that all subclasses will inherit.
  - *You can have an abstract class that you extend and implement an interface at the same time.*
```js
abstract class Employee {
  constructor(public first: string, public last: string) {}
  
  abstract getPay(): number; // Simply means that this method needs to exist in sub classes that extend from this class.
  
  greet() {
    console.log("hello");
  }
}

class FullTimeEmployee extends Employee {
  constructor(first: string, last: string, private salary: number) {
    super(first, last);
  }

  getPay() {
    return this.salary;
  }
}

class PartTimeEmployee extends Employee {
  constructor(first: string, last: string, private hourlyRate: number, private hoursWorked: number) {
    super(first, last);
  }

  getPay() {
    return this.hourlyRate * this.hoursWorked;
  }
}

const employee = new Employee(); // Cannot create an instance of an abstract class.

const fullTimer = new FullTimeEmployee("Tom", "Jerry", 3);
fullTimer.getPay(); // 3
fullTimer.greet(); // "hello"

const partTimer = new PartTimeEmployee("Spike", "Nibbles", 2, 3);
partTimer.getPay(); // 6
partTimer.greet(); // "hello"
```

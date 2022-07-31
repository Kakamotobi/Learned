# Interfaces

## Table of Contents
- [What are Interfaces?](#what-are-interfaces)
- [Readonly and Optional Properties](#readonly-and-optional-properties)
- [Interface Methods](#itnerface-methods)
- [Adding New Properties](#adding-new-properties)
- [Extend/Inherit from Other Interfaces](extendinherit-from-other-interfaces)
- [Types vs Interfaces](#types-vs-interfaces)

## What are Interfaces?
- Interfaces serve almost the exact same purpose as type aliases (with a slightly different syntax - no `=` sign) but only for object types.
- Interfaces can be used to create reusable, modular types that describe the *shapes of **objects***.
### Examples
```ts
interface Person {
  name: string;
  age: number;
};

const sayHappyBirthday = (person: Person) => {
  return `Hello, ${person.name}! Congrats on turning ${person.age}!`;
}

sayHappyBirthday({ name: "Tom", age: 3});
```
```ts
interface Point {
  x: number;
  y: number;
}

const pt: Point = { x: 3, y: 2 };
```

## Readonly and Optional Properties
```ts
interface Person {
  readonly id: number;
  first: string;
  last: name;
  nickname?: string;
}

const tom: Person = { first: "Tom", last: "Jerry", nickname: "Terry" };
```

## Interface Methods
- There are a few different ways to indicate methods in an interface.
- On declaration in the interface, it does not include anything about what it does. It simply indicates the types of any parameters and return value.
```ts
interface Person {
  first: string;
  last: string;
  introduceSelf(): string; // This is not a function that has been defined! It just means that this function should return a 'string' type.
  // introduceSelf: () => string; // Alternative syntax.
}

const tom: Person = {
  first: "Tom",
  last: "Jerry",
  introduceSelf: function() { return `Hi. I'm ${this.first}.` },
  // introduceSelf: () => "Hi. I'm Tom.",
}
```
### Interface Method Parameters
```ts
interface Product {
  name: string,
  price: number,
  applyDiscount(discount: number): number,
}

const shoes:Product = {
  name: "Blue Shoes",
  price: 100,
  applyDiscount(amount: number) {
    const newPrice = this.price * (1 - amount);
    this.price = newPrice;
    return this.price;
  },
};
```

## Adding New Properties
- Interfaces can be "reopened" and be added new properties even after describing the interface.
- Use Case
  - Adding something to an interface from a third-party library.
### Example
```ts
interface Person {
  name: string;
}

interface Person { // This is not a redeclaration. It's just a "reopening"/addition.
  age: number;
}

const tom: Person = {
  name: "Tom",
  age: 3,
}
```

## Extend/Inherit from Other Interfaces
- Use the `extends` keyword to inherit from other interfaces.
### Examples
```ts
interface Person {
  name: string;
  age: number;
}

interface Singer extends Person {
  sing(): string;
}

const usher: Singer = {
  name: "Usher",
  age: 43,
  sing() { return `lalalalala` },
}
```
### Extending Multiple Interfaces
- Use `,` to extend from multiple interfaces.
```ts
interface Person {
  name: string;
}

interface Employee {
  readonly id: number;
  email: string;
}

interface Engineer extends Person, Employee {
  level: string;
  skills: string[];
}

const tom: Engineer = {
  name: "Tom",
  id: 231341,
  email: "tom@tom.com",
  level: "junior",
  skills: ["typescript", "css"],
}
```

## Type Aliases vs Interfaces
- Interfaces can only describe the shape of an object literal.
  - cf. Types can describe the shape of any type.
- Interfaces can be reopened and properties can be added on to them.
  - cf. Types cannot add new properties after declaring them.
- Interfaces can inherit from other interfaces.
  - cf. Types do not inherit from other types. They use intersections(`&`).

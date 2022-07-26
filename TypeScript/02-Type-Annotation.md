# Type Annotation
- "This variable is a string."
- "This function should return a boolean."
- "This function accepts two numbers and returns a number."
- "This object must have a property called colors, set to an array of strings."

## Table of Contents
- [Type Inference](#type-inference)
- [Variable Type Annotation](#variable-type-annotation)
- [Function Type Annotation](#function-type-annotation)
  - [Function Parameter Types](#function-parameter-types)
  - [Function Return Value Types](#function-return-value-types)
    - [`void` Return Type](#void-return-type)
    - [`never` Return Type](#never-return-type)
  - [Anonymous Functions](#anonymous-functions)
- [Object Type Annotation](#object-type-annotation)
  - [Excess Properties](#excess-properties)
  - [Type Alias](#type-alias)
  - [Nested Objects](#nested-objects)
  - [Optional Properties](#optional-properties)
  - [Readonly Modifier](readonly-modifier)
  - [Intersection Types](#intersection-types)
- [Array Type Annotation](#array-type-annotation)
  - [Multidimensional Arrays](#multidimensional-arrays)

## Type Inference
- The TypeScript compiler can look at a variable declaration and determine what type should be inferred for the variable's value.
- It can remember a value's type even without providing a type annotation to it, and enforces that type on that value.
- ***Due to type inference, you can choose to simply annotate variables that are initialized first then its value is declared later.***
  - If a value-less variable is initialized without being annotated, its type is set to "any".
### Example
```ts
let num = 3;
num = "three"; // ERROR - Type 'string' is not assignable to type 'number'.
```

## Variable Type Annotation
- Syntax: `let myVar: type = value`
### Examples
```ts
let str: string = "I am a variable";
let num: number = 3;
let isActive: boolean = true;
```
```ts
let mixed: any = "I am a string";
mixed = 3;
mixed = true;
mixed = [];
mixed = {};
mixed();
```

## Function Type Annotation
### Function Parameter Types
- Specify the type of function parameters when defining a function.
#### Examples
```ts
const sayHello = (name: string) => {
  return `Hello, ${name}!`;
}

sayHello("Tom"); // "Hello, Tom!"
sayHello(3); // Argument of type 'number' is not assignable to parameter of type 'string'.
```
```ts
const createUser = (name: string, age: number, isOnline: boolean) => {
  // create user.
}

createUser("Tom", 3, true);
createUser(3, "three", undefined); // Error
```
#### Default Parameters
```ts
const sayHello = (name: string = "stranger") => {
  return `Hello, ${name}!`;
}

sayHello(); // "Hello, stranger!"
```
### Function Return Value Types
- Specify the type of the value returned by a function.
- TypeScript can infer the return value so you do not have to always include it but it is a good idea to do so.
#### Examples
```ts
const sayHello = (name: string): string => {
  return `Hello, ${name}!`;
}
```
```ts
const addNums = (x: number, y: number): number => {
  return x + y;
}

addNums(3,3); // 6
```
#### `void` Return Type
- A return type for functions that do not return anything.
  - i.e. void of any data.
  - `void` returns `undefined` or `null`.
- TypeScript can infer `void` types as well but will sometimes require you to annotate a function with `void`.
##### Example
```ts
const print(msg: string): void {
  console.log(msg);
}
```
#### `never` Return Type
- A return type for functions that never return a value.
- Use Cases
  - A function that never finishes executing.
  - A function that terminates execution of a program.
  - A function that always throws an exception.
##### Examples
```ts
const gameLoop = (): never => {
  while (true) {
    console.log("Game Loop Running!");
  }
}
```
```ts
const giveError = (msg: string) => {
  throw new Error(msg);
}
```
### Anonymous Functions
- TypeScript infers the type by looking at the context so there is no need to specify the type in most cases.
#### Example
```ts
const colors = ["red", "orange", "yellow"];

colors.map(color => color.toUpperCase());
colors.map(color => color.push("?")); // Property 'push' does not exist on type 'string'.
```

## Object Type Annotation
- Accessing a property that is not defined, or performing operations without keeping types in mind will throw errors.
### Examples
- **Defining an object.**
  ```ts
  let coordinate: {x: number, y: number} = { x: 3, y: 2 };
  ```
- **Object as a parameter to a function.**
  ```ts
  const printName = (name: { first: string, last: string }) => {
    return `Name: ${first} ${last}`;
  }

  printName({first: "Tom", last: "Jerry"}); // Name: Tom Jerry
  ```
- **Object as a return type for a function.**
  ```ts
  const randomCoordinate = (): { x: number, y: number } => {
    return { x: 3, y: 2 };
  }
  ```
### Excess Properties
- **Passing in extra properties that were not declared will result to an error.**
  - TypeScript simply checks to see if the mentioned properties are there.
  - *For an inline object literal, TypeScript additionally checks if ONLY the mentioned properties are there.*
  ```ts
  const printName = (name: { first: string, last: string }) => {
    return `Name: ${first} ${last}`;
  }

  printName({first: "Tom", last: "Jerry", age: 3}); // Error
  ```
- **However, passing in an object does not result to an error.**
  ```ts
  const person = { first: "Tom", last: "Jerry", age: 3, isAlive: true };
  printName(person); // No Error
  ```
### Type Alias
- It is possible to declare object type annotation separately in a type alias (the desired shape of the object).
- This allows for more readable and reusable code.
#### Example
```ts
type Point = {
  x: number;
  y: number;
}

let coordinate: Point = { x: 3, y: 2 };

const randomCoorindate(): Point {
  return { x: Math.random(), y: Math.random() };
}

const doubleCoord = (point: Point): Point => {
  return { x: point.x * 2, y: point.y * 2};
}
```
### Nested Objects
- In complicated nested objects, type aliases could be useful.
```ts
const describePerson = (person: {
  name: string;
  age: number;
  parentNames: {
    mum: string;
    dad: string;
  }
}) => {
  return `Person: ${person.name}, Age: ${person.age}, Parents: ${person.parentNames.mum}, ${person.parentNames.dad}`;
}

describePerson({ name: "Tom", age: 3, parentNames: { mum: "Jerry", dad: "Spike" }});
```
### Optional Properties
- Use the `?` syntax to indicate an optional property.
```ts
type Point = {
  x: number;
  y: number;
  z?: number;
}

const myPoint: Point = { x: 3, y: 2, z: 1 };
const myPoint: Point = { x: 3, y: 2 };
```
### Readonly Modifier
- The `readonly` keyword marks properties on an object read-only.
```ts
type User = {
  readonly id: number;
  username: string;
}

const user: User = {
  id: 32323,
  username: "Tom"
}

user.id = 11111; // Cannot assign to 'id' because it is a read-only property.
```
- **Readonly on Properties that are Objects**
  - Readonly objects (object literal, arrays, etc.) are still mutable (because they are reference types). However, they cannot be reassigned entirely.
### Intersection Types
- A new type is created by intersecting (combining) existing types.
  - This new type includes all features of the existing types.
```ts
type Circle = {
  radius: number;
}

type Colorful = {
  color: string;
}

type ColorfulCircle = Circle & Colorful;

const smileyFace: ColorfulCircle = {
  radius: 5,
  color: "yellow"
}
```
- It is also possible to add on to the intersecting types.
  ```ts
  type Cat = {
    numLives: number;
  }

  type Dog = {
    breed: string;
  }

  type CatDog = Cat & Dog & {
    age: number;
  };

  const tomSpike: CatDog = {
    numLives: 7,
    breed: "Bulldog",
    age: 3
  }
  ```

## Array Type Annotation
- Annotate array types using a type annotation followed by empty array brackets.
- Types are not limited to primitives.
  - Custom types (type aliases) can be used to annotate an array.
### Examples
- Arrays of one type.
  ```ts
  const strArr: string[] = ["hello", "tom"];
  // OR
  const strArr: Array<string> = ["hello", "tom"];
  ```
  ```ts
  const numArr: number[] = [1, 2, 3, 4, 5];
  // OR
  const numArr: Array<number> = [1, 2, 3, 4, 5];
  ```
- Array of type "always empty array".
  ```ts
  const arr: [] = [];
  
  arr.push(1); // Argument of type 'number' is not assignable to parameter of type 'never'.
  ```
- Array of Type Alias
  ```ts
  type Point = {
    x: number;
    y: number
  }
  
  const coords: Point[] = [];
  coords.push({ x: 3, y: 2 }); 
  coords.push({ x: 5 }); // Property 'y' is missing in type '{ x: number; }' but required in type 'Point'.
  ```
### Multidimensional Arrays
- **2-Dimensional Array**
  ```ts
  // 2D array of strings
  const board: string[][] = [["X", "O", "X"], ["X", "O", "X"], ["X", "O", "X"]];
  ```
- **3-Dimensional Array**
  ```ts
  // 3D array of numbers
  const demo: number[][][] = [[[1]], [[2]], [[3]]];
  ```

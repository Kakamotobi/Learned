# Type Annotation
- "This variable is a string."
- "This function should return a boolean."
- "This function accepts two numbers and returns a number."
- "This object must have a property called colors, set to an array of strings."

## Table of Contents
- [Type Inference](#type-inference)
- [Type Annotation for Variables](#type-annotation-for-variables)
- [Type Annotation for Functions](#type-annotation-for-functions)
  - [Function Parameter Types](#function-parameter-types)
  - [Function Return Value Types](#function-return-value-types)
    - [`void` Return Type](#void-return-type)
    - [`never` Return Type](#never-return-type)
  - [Anonymous Functions](#anonymous-functions)

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

## Type Annotation for Variables
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

## Type Annotation for Functions
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

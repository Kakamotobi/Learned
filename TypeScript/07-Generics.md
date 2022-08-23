# Generics

## Table of Contents
- [What are Generics?](#what-are-generics)
- [Some Built-in Generics](#some-built-in-generics)

## What are Generics?
- Allows us to define reusable functions and classes that work with multiple types rather than a single type.
  - Accept any type (`T`) and return the same type.
- Generic Example
  ```ts
  function wrapInArray<T>(element: T): T[] {
    return [element];
  }
  ```
- cf. Union Type
  - By this approach, it is possible to receive a number as an argument and return a string.
  ```ts
  function doSomething(thing: number | string): number | string {
    // 
  }
  ```

## Some Built-in Generics
### Type Interfaces
- Type interfaces accept any (`T`) types and returns the specified type.
  - EX: if you specify `Array<string>`, it will return an array of strings.
  - Hover over to see `interface Array<T>`.
- Example
  - The interface `Array` receives `T` values and returns an array of strings.
  ```ts
  const nums: Array<number> = [];

  const strings: Array<string> = [];
  ```
### `.querySelector()`
- `.querySelector()` is by default set to an ordinary `Element` or `null`.
#### Example
```ts
const inputEl = document.querySelector("#username");

inputEl.value; // Property 'value' does not exist on type 'Element'.
```
```ts
const inputEl = document.querySelector<HTMLInputElement>("#username");

inputEl.value = "Blahblah"; // Object is possibly 'null'.
inputEl?.value = "Blahblah";
```

# Generics

## Table of Contents
- [What are Generics?](#what-are-generics)
- [Some Built-in Generics](#some-built-in-generics)
- [Writing Generics](#writing-generics)
- [Inferred Generics](#inferred-generics)
- [Generics in `.tsx` Files](#generics-in-tsx-files)

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

## Writing Generics
### Example 1
- Identity Function: accepts some argument and returns that argument.
  ```ts
  function numberIdentity(item: number): number {
    return item;
  }

  function stringIdentity(item: string): string {
    return item;
  }
  ```
- The above can be written as a Generic.
  - Using this generic function, TS knows to return the same type that is received as an argument.
  ```ts
  function identity<Type>(item: Type): Type {
    return item;
  }

  identity<string>("tom");
  identity<string>(3); // Argument of type 'number' is not assignable to parameter of type 'string'.
  ```
### Example 2
```ts
// List of type T.
function getRandomElement<T>(list: T[]): T {
  const randIdx = Math.floor(Math.random() * list.length);
  return list[randIdx];
}

getRandomElement<string>(["a", "b", "c"]);
getRandomElement<number>([1, 2, 3]);
```

## Inferred Generics
- TS does not always have to be given the parameter type as it will infer it by looking at the argument.
```ts
getRandomElement(["a", "b", "c"]);

// same as: getRandomElement<string>(["a", "b", "c"]);
```

## Generics in `.tsx` Files
- TS can get confused when a generic function is declared in arrow syntax.
  ```tsx
  const getRandomElement = <T>(list: T[]): T => { // Leads to TS error.
    // ...
  }
  ```
- To solve this, add a trailing comma to the parameter type.
  ```tsx
  const getRandomElement = <T,>(list: T[]): T => { // Leads to TS error.
    // ...
  }
  ```

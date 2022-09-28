# Generics

## Table of Contents
- [What are Generics?](#what-are-generics)
- [Some Built-in Generics](#some-built-in-generics)
- [Writing Generics](#writing-generics)
- [Inferred Generics](#inferred-generics)
- [Generics in `.tsx` Files](#generics-in-tsx-files)
- [Generics with Multiple Types](#generics-with-multiple-types)
- [Adding Type Constraints](#adding-type-constraints)
- [Generics with Default Type Parameters](#generics-with-default-type-parameters)
- [Writing Generic Classes](#writing-generic-classes)

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
  - **Using this generic function, TS knows to return the same type that is received as an argument.**
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

## Generics with Multiple Types
```ts
function merge<T, U>(object1: T, object2: U) { // return type `T & U` is inferred.
  return {
    ...object1,
    ...object2
  }
}

const obj = merge({ name: "kakamotobi" }, { pets: ["tom", "jerry"] }; // { name: string; } & { pets: string[] }
```

## Adding Type Constraints
- `T` and `U` are any type. Therefore, TS does not complain about `merge({ name: "tom" }, 3)`.
  ```ts
  function merge<T, U>(object1: T, object2: U) { // return type `T & U` is inferred.
    return {
      ...object1,
      ...object2
    }
  }
  ```
- To constraint to a particular type, user the `extends` keyword.
  ```ts
  function merge<T extends object, U extends object>(object1: T, object2: U) { // return type `T & U` is inferred.
    return {
      ...object1,
      ...object2
    }
  }
  ```
  ```ts
  interface Length {
    length: number;
  }
  
  // Generic
  function printDoubleLength<T extends Length>(thing: T): number {
    return thing.length * 2;
  }
  
  // No Generic
  function printDoubleLength(thing: Length): number {
    return thing.length * 2;
  }
  ```

## Generics with Default Type Parameters
```ts
function makeEmptyArray<T = number>(): T[] {
  return [];
}

const default = makeEmptyArray(); // number[]
const strings = makeEmptyArray<string>(); // string[]
```

## Writing Generic Classes
- Define a class with a generic type and use that type within the class.
```ts
interface Song {
  title: string;
  artist: string;
}

interface Video {
  title: string;
  creator: string;
  resolution: string;
}

class Playlist<T> {
  public queue: T[] = [];
  
  add(el: T) {
    this.queue.push(el);
  }
}

const songsPlaylist = new Playlist<Song>();
songs.add({ title: "songtitle", artist: "songartist" });

const videosPlaylist = new Playlist<Video>();
videos.add({ title: "videotitle", creator: "videocreator", resolution: "resolution" });
```

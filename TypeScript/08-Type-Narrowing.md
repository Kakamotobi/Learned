# Type Narrowing

## Table of Contents
- [What is Type Narrowing?](#what-is-type-narrowing)
- [`typeof` Guards](#typeof-guards)
- [Truthiness Type Guards](#truthiness-type-guards)
- [Equality Type Guards](#equality-type-guards)
- [`in` Operator](#in-operator)
- [`instanceof` Operator](#instanceof-operator)

## What is Type Narrowing?
- Narrowing down a general type, such as a union type, to a more precise type.

## `typeof` Guards
- **Refers to first doing a type check before working with a value.**
- This works well for primitive types but is not very helpful for objects such as arrays.
### Example
```ts
// Triple value if number, Repeat three times if string.
function triple(value: number | string) {
  if (typeof value === "number") {
    return value * 3;
  } else if (typeof value === "string") {
    return value.repeat(3);
  }
}
```

## Truthiness Type Guards
- **Refers to first checking a value for being truthy or falsy before working with it.**
### Example
```ts
// const printLetters = (word?: string) => {
const printLetters = (word: string | null) => {
  if (!word) console.log("No word was provided.");
  else word.forEach(letter => console.log(letter);
}
```

## Equality Type Guards
- **Refers to comparing types to each other before working with a value.**
### Example
```ts
const someFunc = (x: string | boolean, y: string | number) => {
  if (x === y) {
    x.toUpperCase();
    y.toLowerCase();
  } else {
    console.log(x);
    console.log(y);
  }
}
```

## `in` Operator
- **Refers to using the `in` operator to check if a value exists in an object according to its type alias(es) before working with it.**
### Examples
```ts
type Cat = { meow: () => void };
type Dog = { woof: () => void };

const talk = (creature: Cat | Dog) => {
  if ("meow" in creature) console.log(creature.meow());
  else console.log(creature.woof());
}

const spike: Dog = { woof: () => "WOOF!!!" };
talk(spike); // WOOF!!!
```
```ts
interface Movie {
  title: string,
  duration: number
}

interface TVShow {
  title: string,
  numEpisodes: number,
  episodeDuration: number
}

getTotalRuntime(media: Movie | TVShow) {
  if ("duration" in media) console.log(media.duration);
  else console.log(media.numEpisodes * media.episodeDuration);
}

getTotalRuntime({ title: "Avatar", duration: 162 }); // 162
```

## `instanceof` Operator
- **Refers to using the `instanceof` operator to check if a value is an instance of something before working with it.**
- Usually used when `typeof` won't work (Ex: using classes).
### Examples
```ts
const printFullDate(date: Date | string) {
  if (date instanceof Date) return date.toUTCString();
  else return new Date(date).toUTCString();
}
```
```ts
class User {
  constructor(public username: string) {}
}

class Company {
  constructor(public name: string) {}
}

function printName(entity: User | Company) {
  if (entity instanceof User) console.log(entity.username);
  else console.log(entity.name);
}
```

# Type Narrowing

## Table of Contents
- [What is Type Narrowing?](#what-is-type-narrowing)
- [`typeof` Guards](#typeof-guards)
- [Truthiness Type Guards](#truthiness-type-guards)
- [Equality Type Guards](#equality-type-guards)
- [`in` Operator](#in-operator)
- [`instanceof` Operator](#instanceof-operator)
- [Type Predicates](#type-predicates)
- [Discriminated Unions](#discriminated-unions)
- [Exhaustiveness Checking with `never`](#exhaustiveness-checking-with-never)

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

## Type Predicates
- Type Predicates are features specific to TypeScript.
- **TypeScript allows us to write custom functions that can narrow the type of a value, and return a Type Predicate.**
  - `parameterName is Type`
- Used to tell TypeScript whether a value is of a particular type.
### Example
```ts
// Function to tell whether the value is of type Dog or not.
const isDog(pet: Dog | Cat): pet is Dog {
  return (pet as Dog).woof !== undefined; // true means the type predicate(`pet is Dog`) is true.
}

let pet = getAnimal();
// `pet` gets passed to `isCat` to narrow the type.
if (isDog(pet)) pet.woof();
else pet.meow();
```

## Discriminated Unions
- **Creating a literal property that is common across multiple types, which can then be used to narrow the type.**
  - i.e. creating different types that share a common property, which will be a literal value type.
  - Add a "discriminant" property (Ex: `kind`, `type`, `__type`) that all the types/interfaces will have in common but with different literal values.
### Examples
```ts
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}
```
```ts
interface Rooster {
  name: string;
  weight: number;
  age: number;
  kind: "rooster";
}

interface Cow {
  name: string;
  weight: number;
  age: number;
  kind: "cow";
}

interface Pig {
  name: string;
  weight: number;
  age: number;
  kind: "pig";
}

type FarmAnimal =  Rooster | Cow | Pig;

function getFarmAnimalSound(animal: FarmAnimal) {
  switch(animal.kind) {
    case("rooster"):
      return "Cockadoodledoo!";
    case("cow"):
      return "Moo!";
    case("pig"):
      return "Oink!";
  }
}

const chickenLittle: Rooster = { name: "Chicken Little", weight: 2, age: 3, kind: "rooster" };
getFarmAnimalSound(chickenLittle);
```

## Exhaustiveness Checking with `never`
- Make sure that we have exhausted all possible options.
- Often used for indicating that you are not properly handling a case.
### Example
```ts
function getFarmAnimalSound(animal: FarmAnimal) {
  switch(animal.kind) {
    case("rooster"):
      return "Cockadoodledoo!";
    case("cow"):
      return "Moo!";
    case("pig"):
      return "Oink!";
    default:
      // Point is to never make it here, if all cases were handled correctly.
      const _exhaustiveCheck: never = animal; // results to error since we cannot have animal assigned to type `never`.
      return _exhaustiveCheck;
  }
}

type FarmAnimal = Rooster | Cow | Pig | Sheep;

// This interface is not being handled in `getFarmAnimalSound` yet.
interface Sheep {
  name: string;
  weight: number;
  age: number;
  kind: "sheep";
}

// Therefore, `default` switch will run, informing us that the new addition (Sheep) was not handled.
const baaBaaBlackSheep: Sheep = { name: "Baa Baa Black Sheep", weight: 3, age: 5, kind: "sheep" };
getFarmAnimalSound(baaBaaBlackSheep); // Type 'Sheep' is not assignable to type 'never'.
```

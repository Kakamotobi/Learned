# Newer Features

## Table of Contents
- [Default Function Parameters](#default-function-parameters)
- [Spread](#spread)
- [Rest Parameters](#rest-parameters)
- [Destructuring](#destructuring)

## Default Function Parameters
- Sets the default value for a parameter.
- Syntax: `(parameter = defaultValue)`
- *If there are multiple parameters, make sure to have the ones with default values come after those that do not have default values.*
### Example
```
function rollDie(numsides = 6) {
  return Math.floor(Math.random * numsides) +1;
}
```

## Spread
> Allows an iterable such as an array to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be extended in places where zero or more key-value pairs (for object literals) are expected.
- Syntax: `...`
### Spread in Function Calls
- Spreading an iterable (array, string, etc.) into a function call into separate arguments.
#### Example
```
const nums = [13, 5, 6, 1, 6, 3, 2, 1551, 534, 543];

Math.max(nums); // returns NaN because an array has been passed in intead of numbers.
Math.max(...nums); // returns 1551
```
### Spread with Array Literals
- Spread the elements of an array and create a new array of the spread elements.
- A way to spread an iterable into arrays.
- *Only one level deep. It will not clone nested objects, arrays.*
#### Example 1
```
const cats = ["Tom", "Tim", "Tam"];
const dogs = ["Spike", "Rusty"]

const allPets = [...cats, ...dogs];
allPets; // returns ["Tom", "Tim", "Tam", "Spike", "Rusty"]

// Same result as:

const allPets = cats.concat(dogs);
```
#### Example 2
```
const cats = ["Tom", "Tim", "Tam"];

const catsCopy = [...cats];

// Same result as:

const catsCopy = cats.slice()
```
### Spread with Object Literals
- Copies all properties from one object into another object literal.
- For duplicate properties, the last one overwrites the preceding ones (order matters!).
- *Object literals can be spread only in object literals (cannot be spread directly into an array).*
- *If an array or string is spread into an object literal, the indices are used as the key, and the array elements as the value.*\
- *Only one level deep. It will not clone nested objects, arrays.*
#### Example
```
const feline = { legs: 4, family: "Felidae" };
const canine = { isFurry: true, family: "Caninae" };

const catDog = { ...feline, ...canine, adorable: true };
catDog; // returns {legs: 4, isFurry: true, family: "Caninae", adorable: true }
```
### Reference
[Spread syntax (...) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

## Rest Parameters
- Use it in the parameter to collect *all* arguments or the *remaining* arguments that have not been matched with a parameter, into an actual array, for which we can have a named parameter that will be the name of the array.
- Syntax: `...`
- Looks like spread but is very different.
  - Almost like the opposite of Spread.
  - Instead of spreading data out, Rest collects things down into a single array.
- *Can be used in arrow functions.*
### Example 1 - all arguments
```
function sum(...nums) { // Every argument is combined into an array called nums.
  console.log(nums); // nums is now an actual array.
  return nums.reduce((total, currVal) => total + currVal);
}
```
### Example 2 - all remaining arguments
```
function raceResults(gold, silver, ...everyoneElse) {
  console.log(`Gold Medal Goes To: ${gold}`);
  console.log(`Silver Medal Goes To: ${silver}`);
  console.log(`And thanks to everyone else: ${everyoneElse}`);
}
```
### The Arguments Object (not Rest)
- The `arguments` object is an array-like object that has a length property but does not have array methods.
  - ***To use array methods, we have to turn it into an actual array by using the Spread operator.***
    - Ex: `const argsArr = [...arguments]`
- Contains ALL the arguments/inputs passed in to the function.
  - There is no functionality provided for collecting remaining arguments; even if parameters have been defined.
- Available inside every function but not available inside of arrow functions (use rest parameters for arrow functions).
#### Example
```
function sum () {
  const argsArr = [...arguments]
  return argsArr.reduce((total, currVal) => {
    return total + currVal
  })
}
```

## Destructuring
-A short,clean syntax to "unpack" 1) values from arrays, and 2) properties from objects, into distinct variables.
### Destructuring Arrays
- Unpack based off of the index.
#### Example
```
const raceResults = [
  "Eliud Kipchoge",
  "Feyisa Lelisa",
  "Galen Rupp",
  "Ghirmay Ghebreslassie",
  Alphonce Simbu",
  "Jared War"
];

const [gold, silver, bronze] = raceResults; // gold = "Eliud Kipchoge", silver = "Feyisa Lelisa", bronze = "Galen Rupp"
const [first, , , fourth] = raceResults; // use commas to skip indices.
const [winner, ...participants] = raceResults; // use Rest to store the remaining in an array variable.
```
### Destructuring Objects
- Unpack based off of the name of the property.
#### Example
```
const runner = {
  first: "Eliud",
  last: "Kipchoge",
  country: "Kenya",
  title: "Elder of the Order of the Golden Heart of Kenya"
}

const {first, last} = runner;
const {country: nation} = runner; // makes a variable called "nation" based off of the value found in the "country" key.
const {first, last, ...other} = runner; // use Rest to store the remaining properties in a new object variable.
```
### Nested Destructuring
- Extract nested arrays and objects into individual variables.
#### Example
```
const results = [{
    first: "Eliud",
    last: "Kipchoge",
    country: "Kenya",
  },
  {
    first: "Feyisa",
    last: "Lelisa",
    country: "Ethiopia",
  },
  {
    first: "Galen",
    last: "Rupp",
    country: "United States"
  }
]

const [, {country}] = results; // Ethiopia

// More Readable Way
const [, silverMedal] results;
const {country} = silverMedal;
```
### Destructuring Parameters
- Unpack values from the arguments that have been passed in.
#### Example - objects
```
const runner = {
  first: "Eliud",
  last: "Kipchoge",
  country: "Kenya",
  title: "Elder of the Order of the Golden Heart of Kenya"
}

function print({first, last, title}) {
  console.log(`${first} ${last}, ${title})
}
print(runner); // Eliud Kipchoge, Elder of the Order of the Golden Heart of Kenya

// OR

function print(person) {
  const {first, last, title} = person;
  console.log(`${first} ${last}, ${title})
}
print(runner); // Eliud Kipchoge, Elder of the Order of the Golden Heart of Kenya
```
#### Example - arrays
```
const response = [
  "HTTP/1.1",
  "200 OK",
  "application/json",
]

fumnction parseResponse([protocol, statusCode, contentType]) {
  console.log(`Status: ${statusCode}`)
}
parseResponse(response); // Status: 200 OK

// OR

function parseResponse(response) {
  const [protocol, statusCode, contentType] = response;
  console.log(`Status: ${statusCode}`)
}
  
```

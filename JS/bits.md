# Random Bits

## Table of Contents
- [Default Parameters](#default-parameters)
- [Spread](#spread)
- [Rest Parameters](#rest-parameters)
- [Shorthand for Accessing Objects from API Objects](#shorthand-for-accessing-objects-from-api-objects)

## Default Parameters
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
#### Example
```
const cats = ["Tom", "Tim", "Tam"];
const dogs = ["Spike", "Rusty"]

const allPets = [...cats, ...dogs];
allPets; // returns ["Tom", "Tim", "Tam", "Spike", "Rusty"]
```
### Spread with Object Literals
- Copies properties from one object into a new object literal.
- *If an array or string is spread into an object literal, the indices are used as the key, and the array elements as the value.*
#### Example
```
const feline = { legs: 4, family: "Felidae" };
const canine = { isFurry: true, family: "Caninae" };

const catDog = { ...feline, ...canine };
catDog; // returns {legs: 4, isFurry: true, family: "Caninae" }
```
### Reference
[Spread syntax (...) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

## Rest Parameters
- Use it in the parameter to collect the rest of the argument values into an actual array for which we can have a named parameter that we can call out first.
- Syntax: `...`
  - Looks like spread but is very different.
- Notes
  - The arguments object is an array-like object that has a length property but does not have array methods.
  - Contains all the arguments/inputs passed in to the function.
  - Available inside every function but not avilable inside of arrow functions (use rest parameters).
### Example 1
```
function sum(...nums) {
  console.log(nums); // nums is now an actual array.
  return nums.reduce((total, el) => total + el);
}
```
### Example 2
```
function raceResults(gold, silver, ...everyoneElse) {
  console.log(`Gold Medal Goes To: ${gold}`);
  console.log(`Silver Medal Goes To: ${silver}`);
  console.log(`And thanks to everyone else: ${everyoneElse}`);
}
```

## Shorthand for Accessing Objects from API Objects
### Destructuring
```
// pull out temp, temp_min, temp_max from data.main object and create a dom object out of them.
const {temp, temp_min, temp_max} = data.main;
```

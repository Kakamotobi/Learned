# Useful "Tricks"

## Table of Contents
- [Remainder/Modulo (%) Operator](#remaindermodulo--operator)
- [Sorting Object Keys by their Values](#sorting-object-keys-by-their-values)
- [Accessing a Value (two levels deep) of a Potentially Non-existent Property](#accessing-a-value-two-levels-deep-of-a-potentially-non-existent-property)
- [Replacing All Occurrences of a Substring in a String](#replacing-all-occurrences-of-a-substring-in-a-string)

## Remainder/Modulo (%) Operator
> Returns the remainder left over when one operand is divided by a second operand. It always takes the sign of the dividend.
- Essentially, reduce the first operand to a maximum size of the second operand - 1.
  - Useful for limiting the range of the outcome.
- Notes
  - If the first operand is less than the second operand, the remainder will just be itself.
  - If the first operand is equal to the second operand, the remainder will be 0.
### Useful Scenarios
#### Even/Odd Number
```js
if (x % 2 === 0) {} // if x is an even number.
if (x % 2 !== 0) {} // if x is an odd number.
```
#### Circular Array / Repeated Looping
- Repeated looping over the contents of the array.
  - When an array fills up (reached the end of the array), wrap back to the beginning and start filling back from the beginning.
  - Ex: allows us to store a certain number of most recent entries in the array.
- `[i % array.length]`
##### Example
- `i % studentN.length` loops the array back to the beginning infinitely.
- *i.e., if there is an array of 8 items, and we are currently on item 2 and we skip along 10 items, we want to end up back at slot 4, and not a non-existent slot 12.*
```js
function solution(answers) {
    const student1 = [1, 2, 3, 4, 5];
    const student2 = [2, 1, 2, 3, 2, 4, 2, 5];
    const student3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];
    
    let student1Tally = 0;
    let student2Tally = 0;
    let student3Tally = 0;
    
    for (let i = 0; i < answers.length; i++) {
        if (student1[i % student1.length] === answers[i]) {
            student1Tally++;
        }
        if (student2[i % student2.length] === answers[i]) {
            student2Tally++;
        }
        if (student3[i % student3.length] === answers[i]) {
            student3Tally++;
        }
    }
    
    const answer = [];
    
    const max = Math.max(student1Tally, student2Tally, student3Tally);
    
    if (student1Tally === max) { answer.push(1) };
    if (student2Tally === max) { answer.push(2) };
    if (student3Tally === max) { answer.push(3) };
    
    return answer;
}
```
### Reference
[Remainder (%) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Remainder)  
[Looping infinitely around an array in JavaScript - Ben Frain](https://benfrain.com/looping-infinitely-around-an-array-in-javascript/)

---

## Sorting Object Keys by their Values
```js
const obj = {
    1: 0.13,
    2: 0.43,
    3: 0.50,
    4: 0.50,
    5: 0
}

const keys = Object.keys(obj); // ["1", "2", "3", "4", "5"]

keys.sort((a,b) => obj[a] - obj[b]); // Sort in ascending order of values.

console.log(keys); // ["5", "1", "2", "3", "4"]
```
### Reference
[sorting - Sort hash ascending by value using sortBy in javascript - Stack Overflow](https://stackoverflow.com/questions/33572347/sort-hash-ascending-by-value-using-sortby-in-javascript)  

---

## Accessing a Value (two levels deep) of a Potentially Non-existent Property
### Optional Chaining (`?.`)
> The optional chaining operator (?.) enables you to read the value of a property located deep within a chain of connected objects without having to check that each reference in the chain is valid. | MDN
### Alternatives
- To avoid throwing an Error, you need to add checks for each level/nest.
- Ex: `obj && obj.level1 && obj.level1.level2 && obj.level1.level2.level3`.
### Examples
#### Example 1 - array
```js
const arr = ["one", "two", "three", "four", "five"];

arr[9]; // undefined
arr[9][0]; // Uncaught TypeError: Cannot read properties of undefined (reading '0')
```
```js
// Optional Chaining (ES2020)
arr[9]?.[0]; // undefined

// Alternative 1
let temp;
if (arr[9] && arr[9][0]) temp = arr[9][0]; // undefined

// Alternative 2
let temp = arr[9] ? arr[9][0] : "something else"; // undefined
```
#### Example 2 - object
```js
const obj = {
  "1": [0,1,2,3],
  "2": [2,4],
  "3": [5]
}

obj[9]; // undefined
obj[9][2]; // Uncaught TypeError: Cannot read properties of undefined (reading '2')
obj[9].includes(0); // Uncaught TypeError: Cannot read properties of undefined (reading 'includes')
```
```js
// Optional Chaining (ES2020)
obj[9]?.[2]; // undefined
obj[9]?.includes(0); // undefined

// Alternative 1
let temp;
if (obj[9] && obj[9][2]) temp = arr[9][2]; // undefined
if (obj[9] && obj[9].includes(0)) temp = obj[9].includes(0); // undefined

// Alternative 2
let temp = obj[9] ? obj[9][2] : "something else"; // undefined
let temp = obj[9] ? obj[9].includes(0) : "something else"; // undefined
```
### Reference
[Optional chaining (?.) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)  

---

## Replacing All Occurrences of a Substring in a String
### Option 1: Regex
- Use regex for `replace` because we want *all* occurrences of a substring.
```js
const str = "Hello, kakamoto's account is @kakamoto."
const newStr = str.replace(/kakamoto/g, "kakamotobi");
console.log(newStr); // "Hello, kakamotobi's account is @kakamotobi"
```
### Option 2: `String.prototype.split()` and `Array.prototype.join()`
```js
const str = "Hello, kakamoto's account is @kakamoto."
const newStr = str.split("kakamoto").join("kakamotobi);
console.log(newStr); // "Hello, kakamotobi's account is @kakamotobi"
```
### Option 3: `String.prototype.replaceAll()`
```js
const str = "Hello, kakamoto's account is @kakamoto."
const newStr = str.replaceAll("kakamoto", "kakamotobi");
// or str.replaceAll(/kakamoto/g, "kakamotobi");
console.log(newStr); // "Hello, kakamotobi's account is @kakamotobi"
```
### Reference
[replaceAll in JavaScript | Medium](https://medium.com/geekculture/replaceall-in-javascript-b61f4e94f028)  

---
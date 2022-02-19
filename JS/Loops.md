# Loops

## Definition
A sequence of instructions that is continually repeated until a certain condition is met.

## for Loop
- Syntax: `for (initial expression; condition; increment expression) {statement}`
  - `initial expression`: executed once before the code is run.
  - `condition`: the loop continues as long as this condition is true.
  - `increment expression`: executed after each iteration.
- Ex: `for (let i = 0; i < arr.length; i++) {console.log(array[i])}`
- Nested `for` loop
  - For every single iteration of the outer loop, the inner loop will have a full cycle of iterations.
  - Ex:
  ```js
  for (let i = 0; i < arr1.length; i++) {
    const arr2 = arr1[i];
    for (let j = 0; j < arr2.length; j++) {
      console.log(arr2[j]);
    }
  }
  ```

## while Loop
- Syntax: `while (condition) {statement}`
- As long as the `condition` stays true (checked on every iteration), the loop will continue.
- Useful for when the number of required iterations is unknown/undecided (ex: user input, pw input, game).
- The **`break`** keyword
  - Stops the execution of the loop immediately and continues to run the next codes.
  - Useful for when the number of required iterations is unknown/undecided.
- Ex:
  ```js
  let n = 0;
  while (n !== 10) {
    n++;
    if (n === 10) {
      break;
    }
  }
  ```

## for...of Loop
- Syntax: `for (variable of iterable) {statement}`
  - `variable` represents the individual elements in the iterable object (ex: arrays, arguments).
  - ***Object literals are NOT iterable. Refer to [Object Methods](#iterating-over-objects-using-object-methods).***
- Creates a loop that iterates over iterable objects (ex: arrays, arguments).
- Executes the statement for each of the element in the iterable object.
- Ex: 
  ```js
  for (let element of arr) {
    console.log(element)
  }
  ```

## for...in Loop
- Syntax: `for (variable in object) {statement}`
  - `variable` represents the individual property
- Iterates the key of the object.
  - To access the value, use the key.
    - Ex:
      ```js
      for (let key in object) {
        console.log(object[key]);
      }
      ```
### Using for...in on an Array
- When looping over an array using for...in, the `variable` represents the *index*.
- However, using for...in to loop over an array is a bad idea because one construct can be different from the other.
  - Ex:
    ```js
    const arr = [];
    arr[5] = "test";

    // for Loop
    for (let i = 0; i < arr.length; i++) console.log(arr[i]); // undefined undefined undefined undefined undefined "test"

    // for...in Loop
    for (let x in arr) console.log(x); // 5
    ```
- Also, the for...in loop will return all enumerable properties, including those with non-integer names and those that are inherited (some libraries may add on to `Array.prototype` properties).
  - Ex:
    ```js
    Array.prototype.blah = 7;
    
    const arr = [1,2,3,4,5];
    
    for (let x in arr) console.log(x); // 0 1 2 3 4 blah
    ```

## Iterating Over Objects using Object Methods
- Convert an object into an array for it to be iterable, and then use `for...of` loop.
- **`Object.keys()`**
  - Returns an array made up of the keys from the object.
  - Insert object's name in the `()`.
- **`Object.values()`**
  - Returns an array made up of the values from the object.
  - Ex:
    ```js
    const testScores = {tom: 90, jerry: 95, spike: 80};
    let total = 0;
    let allScores = Object.values(testScores); // converted the values of the object properties into an array.
    for (let score of allScores) {
      total += score;
    }
    const avg = total / allScores.length;
    avg; // average score.
    ```
- **`Object.entries()`**
  - Returns a nested array of key-value pairs.

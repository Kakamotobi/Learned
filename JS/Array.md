# Array

## Definition
- Ordered collection of values.
- Can have an array of mixed types.

## Reference Types & Equality Testing
- When equality testing with arrays, JS doesn't compare the contents of the arrays. Instead, it compares the references in memory.
- Individual arrays have their own unique reference/memory. So, even if the contents are the same, `arr1 === arr2` is false.
- However, if `arrCopy = arr`, both `arrCopy` and `arr` have the same reference. So, `arrCopy === arrr` is true.

## Array Methods
- **`.push()`**
  - Add value in `()` to the end of an array.
- **`.pop()`**
  - Remove the last element of an array.
- **`.shift()`**
  - Remove the first element of an array.
- **`.unshift()`**
  - Add value in `()` to the start of an array.
- **`.concat()`**
  - Merge two or more arrays to form and return a new array.
- **`.includes()`**
  - Returns `true` or `false` for whether the value in `()` exists in the array or not.
- **`.indexOf()`**
  - Returns the first index of the value in `()` that can be found in the array.
  - Returns `-1` if not found in the array.
  - Can also indicate which index to start looking from (ex: `arr.indexOf("x", 5)`).
- **`.reverse()`**
  - Reverses the order of entries in an array.
- **`.join()`**
  - Creates and returns a new string of entries of an array, including commas.
- **`.slice()`**
  - Copies a portion of an array.
- **`.splice()`**
  - Removes/replaces existing elements in an array and/or add new elements.
  - Syntax: `arr.splice(start[, deleteCount[, item1[, item2[, ...]]]])`
- **`.sort()`**
  - By default, converts all entries into strings and order them according to UTF-16 code unit values.
  - Syntax: `arr.sort([compareFunction])`
    - `compareFunction` defines the sort order.
      - For array of numbers (ascending): `(a, b) => a - b;`
      - For array of objects: `(a, b) => {a.propertyName - b.propertyName;}`

## Array Callback Methods
Accepts a callback function as its argument.
- **`.forEach()`**
  - Executes a function given in `()` once for each element in the array.
- **`.map()`**
  - Executes a function given in `()` once for each element in the array, and also creates a new array with the results.
  - Typically used when we need a portion of our data. Or when we need to transform every element in the starting array and create a new array based upon it.
  - Ex: `arr.map(x => x + 1)`
- **`.find()`**
  - Returns the value of the first element in the array that satisfies the provided testing function.
  - Ex: `let movies = movies.find(movie => { return movie.includes("Bat") })`
- **`.filter()`**
  - Creates a new array of all the elements of the array that passed the test implemented in `()`.
  - Ex: `arr.filter(x => x > 0)`
- **`.every()`**
  - Returns `true` or `false` if all the elements in the array passed the test in `()` or not.
- **`.some()`**
  - Returns `true` or `false` if ANY of the elements in the array passed the test in `()` or not.
- **`.reduce()`**
  - Executes a reducer function (with two parameters: `accumulator`, `currentValue`) on each element of the array, resulting in a single output value.
    - `accumulator`: represents the single output value that will be reduced down to.
      - The reducer function's returned value is assigned to the `accumulator`, which is remembered and updated throughout each iteration.
      - Can set a starting point for the accumulator.
        - Ex: `arr.reduce((accumulator, x) => accumulator + x, 10)`
    - `currentValue`: represents each individual element in the array.
  - Array of Numbers Ex: `arr.reduce((accumulator, x) => accumulator + x)`
  - Array of Objects Ex:  
    ```
    movies.reduce((bestMovie, currMovie) => {
      if (currMovie.score < bestMovie.score) {
        return bestMovie;
      } return currMovie;
    });
    ```

## Reference
[Array - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

# Sorting Algorithms

## Table of Contents
- [Sorting](#sorting)
- [JS `Array.prototype.sort()` Method](#js-arrayprototypesort-method)
- [Elementary Sorting Algorithms](#elementary-sorting-algorithms)
  - [Bubble Sort](#bubble-sort)
  - [Selection Sort](#selection-sort)
  - [Insertion Sort](#insertion-sort)

## Sorting
- **The process of rearranging the items in a collection (ex: array, string) so that the items are in some kind of order.**
  - Ex: sorting numbers (ascending, descending), names alphabetically, movies based on revenue.
- Sorting is a very common task, so it is good to know how it works.
  - Even when using the built-in JS sort method, it is important to know what algorithm it is using behind the scenes.
- There are many different ways to sort tihngs, and different techniques have their pros and cons.

## JS `Array.prototype.sort()` Method
- By default converts each item into strings and compares their sequences of UTF-16 code units values.
- Accepts an optional comparator function.
  - The comparator looks at pairs of elements (a and b), and determines their sort order based on the return value.
    - If return value is negative, a should come before b.
    - If return value is positive, a should come after b.
    - If return value is 0, a and b are considered the same.

## Elementary Sorting Algorithms
- Less commonly used because they are less efficient.
### Bubble Sort
- **A sorting algorithm where when sorting in ascending order, the largest values "bubble" up to the top.**
- For each iteration, compare two adjacent values and swap the larger one to the right. Then move on to the next pair and repeat until the largest value reaches the end.
- Not commonly used because it is inefficient except for one use case where it excels.
#### Big O
- **TC: O(n<sup>2</sup>)**
  - Best Case: O(n)
  - Average/Worst Case: O(n<sup>2</sup>)
#### Implementation
```js
function bubbleSort(arr) {
  // Initialize noSwaps variable.
  let noSwaps;
  // For loop for each position in arr (-1 to avoid unnecessary last iteration).
  for (let i = 0; i < arr.length - 1; i++) {
    // For each iteration, set noSwaps to true.
    noSwaps = true;
    // For loop to compare two adjacent values.
    for (let j = 0; j < arr.length - 1 - i; j++) {
      // If first value is greater than second value. For descending order, check if arr[j] < arr[j+1].
      if (arr[j] > arr[j+1]) {
        // Swap the two values.
        [arr[j+1], arr[j]] = [arr[j], arr[j+1]];
        // If a swap was made, update noSwaps.
        noSwaps = false;
      }
    }
    // If noSwaps were made, break the loop.
    if (noSwaps) break;
    
  }
  return arr;
}
```
### Selection Sort
- **A sorting algorithm where when sorting in ascending order, the lowest value is replaced to be at the front of the array.**
  - Index 0: find the lowest value and replace with the value at index 0.
  - Index 1 and on: find the lowest value from the target index to the end and replace with the value at the target index.
- Similar to bubble sort, but instead of first placing large values into sorted position, it places small values into sorted position one at a time (sorted data accumulates at the beginning).
  - *Selection Sort is better than Bubble Sort in the scenario when you want to minimize swaps.*
#### Big O
- **TC: O(n<sup>2</sup>)**
#### Implementation
```js
function selectionSort(arr) {
  // For loop for each position in arr (-1 to aviod unnecessary last iteration).
  for (let i = 0; i < arr.length - 1; i++) {
    // Initialize indexOfMinVal to the first value of the iteration.
    let indexOfMinVal = i;
    // For loop to find the index of the minimum value.
    for (let j = i + 1; j < arr.length; j++) {
      // If the next item is smaller than the current indexOfMinVal. For descending order, check if arr[j] > arr[indexOfMaxVal].
      if (arr[j] < arr[indexOfMinVal]) {
        // Update the indexOfMinVal to be the index of the next item.
        indexOfMinVal = j;
      }
    }
    // If indexOfMinVal is not the value that it was initialized with (i.e., if it hasn't changed, there's no need to swap).
    if (indexOfMinVal !== i) {
      // Swap the two values (first value of iteration and value at indexOfMinVal).
      [arr[i], arr[indexOfMinVal]] = [arr[indexOfMinVal], arr[i]];
    }
  }
  
  return arr;
}
```
### Insertion Sort


## Reference
[Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)  

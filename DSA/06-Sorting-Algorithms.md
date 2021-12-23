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
- Not commonly used because it is inefficient but works well on nearly sorted data.
#### Big O
- **TC: O(n<sup>2</sup>)**
  - Best Case: O(n)
  - Average/Worst Case: O(n<sup>2</sup>)
- **SC: O(1)**
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
  - Best/Average/Worst Case: O(n<sup>2</sup>)
- **SC: O(1)**
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
- **A sorting algorithm where an unsorted element is placed at its suitable position in the already sorted part of the array.**
- Builds up the sort by gradually creating a larger left portion which is always sorted.
- Situations where insertion sort does well.
  - When an array is mostly/nearly sorted.
  - Doesn't have to have the data all at once. Insertion Sort can receive data while sorting (online algorithm).
#### Big O
- **TC: O(n<sup>2</sup>)**
  - Best Case: O(n)
  - Average/Worst Case: O(n<sup>2</sup>)
- **SC: O(1)**
#### Implementation
```js
// Implementation 1

function insertionSort(arr) {
  // For loop for each index of arr; excluding index 0.
  for (let i = 1; i < arr.length; i++) {
    // The key value for this iteration.
    let key = arr[i];
    // Pointer for sorted portion.
    let j = i - 1;
    // While the preceding element (value in the sorted array) is greater than the current key value. For descending order, check arr[j] < key.
    while (arr[j] > key && j >= 0) {
      // Save the preceding element arr[j] at index j+1, effectively "shifting" it to the right. It is safe to overwrite arr[j+1] because its value is stored in key anyways.
      arr[j+1] = arr[j];
      // Decrement j to preceding index.
      j--;
    }
    // If the right index for key has been found. j+1 because j is currently the index of the value less than key. So, insert key in front of j.
    arr[j+1] = key;
  }
  
  return arr;
}
```
```js
// Implementation 2

function insertionSort(arr) {
  let j;
  for (let i = 1; i < arr.length; i++) {
    let key = arr[i];
    for (j = i - 1; j >= 0 && arr[j] > key; j--) {
      arr[j+1] = arr[j]
    }
    arr[j+1] = key;
  }
  return arr;
}
```

## Reference
[Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)  

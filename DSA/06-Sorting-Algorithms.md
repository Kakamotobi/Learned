# Sorting Algorithms

## Table of Contents
- [Sorting](#sorting)
- [JS `Array.prototype.sort()` Method](#js-arrayprototypesort-method)
- [Elementary Sorting Algorithms](#elementary-sorting-algorithms)
  - [Bubble Sort](#bubble-sort)
  - [Selection Sort](#selection-sort)
  - [Insertion Sort](#insertion-sort)
- [Intermediate Sorting Algorithms](#intermediate-sorting-algorithms)
  - [Merge Sort](#merge-sort)
  - [Quick Sort](#quick-sort)
  - [Radix Sort](#radix-sort)

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
- Do not scale well.
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

## Intermediate Sorting Algorithms
- More complex but faster sorting algorithms.
### Merge Sort
- **Works by decomposing an array into smaller arrays of 0 or 1 elements, then building up a newly sorted array.**
  - Exploits the fact that arrays of 0 or 1 element are always sorted.
  - Divide and Conquer approach. 
- A combination of splitting up, sorting, merging.
- Most merge sort implementations use recursion.
- Process
  - Split up until we end up with sorted arrays (length of 0 or 1).
  - Then merge the sorted arrays step by step until everything is merged back to one array.
#### Big O
- **TC: O(n log n)**
  - Best/Average/Worst Case: O(n log n).
  - If n is 8, it takes 3 splits/decompositions to get single lengthed arrays.
    - log<sub>2</sub>8 = 3.
  - If n is 32, it takes 5 splits/decompositions to get single lengthed arrays.
    - log<sub>2</sub>32 = 5.
  - **As n grows, the number of splits/decompositions needed grows at a rate of log n. Hence, O(log n).**
  - **However, for each split/decomposition, there are O(n) comparisons (when merging).**
    - Ex: merge([3,4,5,8], [1,2,6,7]). As n grows, the `merge` algorithm has time complexity of O(n).
  - **Hence, O(n log n).**
- **SC: O(n)**
#### Implementation
```js
// Function to merge sorted arrays.
// TC: O(n+m), SC: O(n+m)
// Should not modify the parameters passed to it.

// Approach 1

function merge(arr1, arr2) {
  // Initialize empty array.
  let results = [];
  
  // Initialize pointers
  let i = 0;
  let j = 0;
  
  // While i and j are both less than their respective lengths.
  while (i < arr1.length && j < arr2.length) {
    // If value in arr1 is less than value in arr2.
    if (arr1[i] <= arr2[j]) {
      // Push value in arr1 to results.
      results.push(arr1[i]);
      // Increment i.
      i++;
    }
    // If value in arr2 is less than value in arr1.
    else if (arr2[j] < arr1[i]) {
      // Push value in arr2 to results.
      results.push(arr2[j]);
      // Increment j.
      j++;
    }
  }
  
  // While i hasn't reached its end (meaning, j has reached its end first, effectively breaking out of the above while loop).
  while (i < arr1.length) {
    // Loop through remaining values and push them to results.
    results.push(arr1[i]);
    // Increment i.
    i++;
  }
  
  // While j hasn't reached its end (meaning, i has reached its end first, effectively breaking out of the above while loop).
  while (j < arr2.length) {
    // Loop through remaining values and push them to results.
    results.push(arr2[j]);
    // Increment j.
    j++;
  }

  return results;
}

// Approach 2

function merge(arr1, arr2) {
  let results = [];
  let i = 0;
  let j = 0;
  
  while (i < arr1.length || j < arr2.length) {
    // j >= arr2.length accounts for when values in arr2 have all been merged but there are still values in arr1 remaining.
    if (arr1[i] <= arr2[j] || j >= arr2.length) {
      results.push(arr1[i]);
      i++;
    }
    // i >= arr1.length accounts for when values in arr1 have all been merged but there are still values in arr2 remaining.
    else if (arr2[j] < arr[i] || i >= arr1.length) {
      results.push(arr2[j]);
      j++;
    }
  }
  
  return results;
}
```
```js
// Function to merge sort.

function mergeSort(arr) {
  // Base Case: if 
  if (arr.length <= 1) return arr;
  
  // Recursive call on each half of arr until each value is in its own array (i.e., sorted array).
  // Ends up looking like merge([10, 24],[43, 67]) right before the value is returned.
  return merge(mergeSort(arr.slice(0, Math.floor(arr.length / 2))), mergeSort(arr.slice(Math.floor(arr.length / 2))));
}
```
#### Process Example
```js
mergeSort([10,24,67,43]);

// "Left" portion
// mergeSort([10,24])
   // mergeSort([10]) // "left" portion
      // [10]
   // mergeSort([24]) // "right" portion
      // [24]
// merge([10], [24]) // merge "left" and "right" portion
   // [10,24]

// "Right" portion
// mergeSort([67,43])
   // mergeSort([67]) // "left" portion
      // [67]
   // mergeSort([43]) // "right" portion
      // [43]
// merge([67], [43]) // merge "left" and "right" portion
  // [43,67]

// Merge "left" and "right" portion (the very first merge() that has been waiting in the call stack).
// merge([10,24], [43,67])
// [10,24,43,67]
```
### Quick Sort
### Radix Sort

## Reference
[Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)  

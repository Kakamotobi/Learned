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
    // If noSwaps were made, meaning that arr is already sorted, break the loop.
    if (noSwaps) break;
    
  }
  return arr;
}
```
### Selection Sort
- **A sorting algorithm where when sorting in ascending order, the lowest value is replaced to be at the front of the array.**
  - Index 0: find the lowest value and replace with the value at index 0.
  - Index 1 and on: find the lowest value between the target index and the end, then replace it with the value at the target index.
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
  // For loop for each index of arr; starting from index 0.
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
- *Most merge sort implementations use recursion.*
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
// Merge Sort Implementation

function mergeSort(arr) {
  // Base Case: if length of arr is equal to or less than 1, return arr.
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
- **Works by decomposing an array into smaller arrays of 0 or 1 elements, then building up a newly sorted array.**
  - Exploits the fact that arrays of 0 or 1 element are always sorted.
  - Divide and Conquer approach.
- **However, different from merge sort in that, it works by selecting one element (called the "pivot") and finding the index where the pivot should end up in the sorted array.**
  - Ex: if the pivot is the values in the middle, all the values that are lower than that value will be moved to the left and all the values that are greater than that value will be moved to the right (not sorted yet). Then repeat the process for the left and right again until the length of arrays are equal to or less than 1.
  - You know for certain that the pivot is in the right spot.
- The runtime of quick sort depends in part on how one selects the pivot.
  - Ideally, the pivot should be chosen so that it's roughly the median value in the data set that you're sorting.
- *Most quick sort implementations use recursion.*
#### Big O
- **TC: O(n<sup>2</sup>)**
	- Best/Average Case: O(n log n)
	- Worst Case: O(n<sup>2</sup>)
		- If the pivot is the minimum/maximum value and the array is already sorted.
		- Solution: pick a random pivot point or the middle value instead of first or last value.
- **SC: O(log n)**
#### Implementation
```js
// Pivot/Partition Helper Function

function pivotHelper(arr, startIdx = 0, endIdx = arr.length - 1) {
  // Grab the pivot from the start of the arr.
  let pivot = arr[startIdx];
  
  // Initialize pivotIdx to keep track of how many elements are less than pivot (ultimately becomes pivot's correct position).
  let pivotIdx = 0

  // Loop through arr.
  for (let i = 1; i < arr.length; i++) {
  	// If this element is less than the pivot.
  	if (arr[i] < pivot) {
  		// Increment pivotIdx.
  		pivotIdx++;
  		// Swap the current element with the element at the pivot index.
  		[arr[i], arr[pivotIdx]] = [arr[pivotIdx], arr[i]];
  	}
  }

  // Swap the pivot with the number at the pivot index.
  [arr[startIdx],arr[pivotIdx]] = [arr[pivotIdx], arr[startIdx]];

  return pivotIdx;
}
```
```js
// Implementation 1 - In Place

function quickSort(arr, startIdx = 0, endIdx = arr.length - 1) {
	// Base Case: as soon as startIdx and endIdx are equal, we have 1 element in the subarray.
	if (startIdx < endIdx) {
		// Get pivot index.
		let pivotIdx = pivotHelper(arr, startIdx, endIdx);

		// Recursively call pivotHelper on the "left" portion (excl. pivot).
		quickSort(arr, startIdx, pivotIdx - 1)

		// Recursively call pivotHelper on the "right" portion (excl. pivot).
		quickSort(arr, pivotIdx + 1, endIdx);
	}
	return arr;
}
```
```js
// Implementation 2 - Not In Place
// More readable and simple but memory exhaustive.

function quickSort(arr) {
  // Base Case: if array length is equal to or less than 1, return it and push it up the stack.
  if (arr.length <= 1) return arr;
  
  // Initialize pivot.
  let pivot = arr[0];
  // Initialize subarrays.
  let left = [];
  let right = [];
  
  // Loop over passed in array.
  for (let i = 1; i < arr.length; i++) {
    // If value is equal to or less than pivot, push to left array.
    if (arr[i] <= pivot) left.push(arr[i]);
    // Else if value is greater than pivot, push to right array.
    else if (arr[i] > pivot) right.push(arr[i]);
  }
  
  // Recursive call on subarrays, then concatenate the returned sorted arrays.
  return quickSort(left).concat([pivot], right);
}
```
#### Process Example - Implementation 1 (in place)
```js
quickSort([4,6,9,1,2,5,3])
          // pivotHelper(arr, 0, 6) // pivotIdx: 3, arr: [3,1,2,4,9,5,6]
          // quickSort(arr, 0, 3-1) // "left" portion [3,1,2]
                       // pivotHelper(arr, 0, 2) // pivotIdx: 2, arr: [2,1,3,4,9,5,6]
                       // quickSort(arr, 0, 2-1) // [2,1]
                                    // pivotHelper(arr, 0, 1) // pivotIdx: 1, arr: [1,2,3,4,9,5,6]
                                    // quickSort(arr, 0, 1-1) // [1]. Base Case reached. Return [1,2,3,4,9,5,6]
          // quickSort(arr, 2+1, 6) // "right" portion [9,5,6]
                       // pivotHelper(arr, 4, 6) // pivotIdx: 6, arr: [1,2,3,4,6,5,9]
                       // quickSort(arr, 4, 6-1) // [6,5]
                                    // (arr, 4, 5) // pivotIdx: 5, arr: [1,2,3,4,5,6,9]
                                    // quickSort(arr, 4, 5-1) // [5]. Base Case reached. Return [1,2,3,4,5,6,9]
```
### Radix Sort
- **A special sorting algorithm that works on lists of numbers and does not make direct comparisons (ex: is i greater than i+1).**
- It exploits the fact that information about the size of a number is encoded in the number of digits.
	- More digits means a larger number.
- Usually used with anything that can be expressed in numbers (ex: numbers, strings, images).
- Process
	- Group each number in the array based off of the ones digit (ex: 5 from 425).
	- Put them back in a list, keeping the order they are in (not sorted yet).
	- Now group each number based off of the tens digit (ex: 2 from 425).
	- Put them back in a list, keeping the order they are in (not sorted yet).
	- Now group each number based off of the 100s digit (ex: 4 from 425).
	- Put them back in a list, keeping the order they are in.
	- *Repeat this for the number of digits that the largest number has.*
	- Now array is sorted.
- ***Note***
	- Radix Sort (O(n\*k)) can be significantly better than a comparison sort algorithm (O(n log n)).
	- However, if all numbers are distinct, k has to be at least log n because of the way that computers store information. Resulting to O(n log n).
#### Big O
- n: number of things being sorted (length of array).
- k: the number of digits in those numbers (word size).
- **TC: O(n\*k)**
	- Best/Average/Worst Case: O(n\*k)
- **SC: O(n+k)**
#### Implementation
```js
// Helper Functions

// Returns the digit in num at the given place value (ones, tens, hundreds, ...).
function getDigit(num, i) {
	// Divide num by 10^i. Then floor it. Then mod 10.
	// 10^i gives 0, 10, 100, ...
	return Math.floor(Math.abs(num) / Math.pow(10, i)) % 10;
}

// Returns the number of digits in a number.
function digitCount(num) {
	// Edge case
	if (num === 0) return 1;
	// 10 to what power gives us num? Add 1 to it.
	return Math.floor(Math.log10(Math.abs(num))) + 1;
}

// Returns the largest number of digits in an array of numbers.
function mostDigits(nums) {
	let result = 0;
	
	for (let i = 0; i < nums.length; i++) {
		result = Math.max(result, digitCount(nums[i]));
	}
	
	return result;
}
```
```js
function radixSort(arr) {
	// Figure out how many digits the largest number has.
	let mostDigitCount = mostDigits(arr);
	
	// Loop largest number of digits times.
	for (let i = 0; i < mostDigitCount; i++) {
		// For each iteration, create buckets for each digit (0 to 9). An array of 10 subarrays.
		let digitBuckets = Array.from({length: 10}, () => []);
		
		// Loop over each number in arr.
		for (let j = 0; j < arr.length; j++) {
			// Access the ith digit of each number.
			let digit = getDigit(arr[j], i);
			// Push each number in the corresponding bucket based on its ith digit.
			digitBuckets[digit].push(arr[j]);
		}
		
		// After each number is placed in its corresponding bucket, flatten the array.
		arr = [].concat(...digitBuckets);
	}
	
	return arr;
}
```
#### Process Example
```js
radixSort([23,345,5467,12,2345,9852]);
// i = 0 // digitBuckets = [[],[],[12,9852],[23],[],[345,2345],[],[5467],[],[]]
	// arr = [12,9852,23,345,2345,5467]
// i = 1 // digitBuckets = [[],[12],[23],[],[345,2345],[9852],[5467],[],[],[]]
	// arr = [12,23,345,2345,9852,5467]
// i = 2 // digitBuckets = [[12,23],[],[],[345,2345],[5467],[],[],[],[9852],[]]
	// arr = [12,23,345,2345,5467,9852]
// i = 3 // digitBuckets = [[12,23,345],[],[2345],[],[],[5467],[],[],[],[9852]]
	// arr = [12,23,345,2345,5467,9852]
```

## Reference
[Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)  
[visualising data structures and algorithms through animation](https://visualgo.net/en)  

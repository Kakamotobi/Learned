# Practice Problems

## Table of Contents
- [Divide and Conquer](#divide-and-conquer)

## Divide and Conquer
### 1. Sorted Frequency
- Given a sorted integer array `arr` and an integer `num`, write a function that counts the occurrences of `num` in `arr`.
#### Example
```js
sortedFrequency([1,1,2,2,2,2,3], 2) // 4
sortedFrequency([1,1,2,2,2,2,3], 3) // 1
sortedFrequency([1,1,2,2,2,2,3], 1) // 2
sortedFrequency([1,1,2,2,2,2,3], 4) // -1
```
#### Solution
- Find the indices of the first and last occurrence of `num`.
- Subtract the indices and add `1` to find the total occurrences.
- Edge Case:
  - If `num` does not exist in `arr`, return -1.
```js
function sortedFrequency(arr, num) {
  const firstOccurrence = findIdxOf(arr, num, true); // true for if looking for first occurring number.
  const lastOccurrence = findIdxOf(arr, num, false); // false for if looking for last occurring number.
  
  return firstOccurrence === -1 ? -1 : lastOccurrence - firstOccurrence + 1;
}

function findIdxOf(arr, num, isFirstOccurrence) {
  let start = 0;
  let end = arr.length - 1;
  let mid = 0;
  let idx = -1; // Initialize idx to -1 in case num does not exist in arr.
  
  while (start <= end) {
    mid = Math.floor((start + end) / 2);
    
    if (arr[mid] === num) {
      idx = mid; // Sync idx to an index containing num.
      if (isFirstOccurrence) end = mid - 1;
      else if (!isFirstOccurrence) start = mid + 1;
    } else if (arr[mid] < num) {
      start = mid + 1;
    } else {
      end = mid - 1;
    }
  }
  
  return idx;
}
```
### 2. Find Index in Rotated Sorted Array
- Given a sorted integer array `arr` that has been rotated and an integer `num`, write a function that returns the index of `num` in `arr`. If not found, return `-1`.
- Constraints
  - Time Complexity: O(log n)
  - Space Complexity: O(1)
#### Example
```js
findIndexInRotatedSortedArray([3,4,1,2], 4) // 1
findIndexInRotatedSortedArray([6,7,8,9,1,2,3,4], 8) // 2
findIndexInRotatedSortedArray([37,44,66,102,10,22], 14) // -1
findIndexInRotatedSortedArray([11,12,13,14,15,16,3,5,7,9], 16) // 5
```
#### Solution
- Key Information
  - A "rotated" sorted array means that the array can be divided into two sorted arrays.
  - The first number is always greater than the last number.
- Find the point of rotation.
- Perform binary search on each sorted subset with the point of rotation.
```js
function findIndexInRotatedSortedArray(arr, num) {
  const rotatedIndex = findRotatedIndex(arr);
  
  // Check the left sorted subset.
  const leftSortedSubset = binarySearchWithinIndices(arr, num, 0, rotatedIndex - 1);
  if (leftSortedSubset !== -1) return leftSortedSubset;
  
  // Check the right sorted subset.
  const rightSortedSubset = binarySearchWithinIndices(arr, num, rotatedIndex, arr.length - 1);
  if (rightSortedSubset !== -1) return rightSortedSubset;
  
  return -1;
}

function findRotatedIndex(arr) {
  let start = 0;
  let end = arr.length - 1;
  let mid = 0;

  // Objective: move `start` to the end of the left sorted subset and `end` to the start of the right sorted subset.
  while (start < end) {
    mid = Math.floor((start + end) / 2);
    
    // If end and start are 1 index apart, return end (the index of the start of the right sorted subset).
    if (end - start === 1) return end;
    
    // If arr[mid] is in the left sorted subset.
    if (arr[mid] >= arr[start]) start = mid;
    // Else if arr[mid] is in the right sorted subset.
    else if (arr[mid] <= arr[end]) end = mid;
  }
}

function binarySearchWithinIndices(arr, num, start, end) {
  let mid = 0;
  while (start <= end) {
    mid = Math.floor((start + end) / 2);
    
    if (arr[mid] === num) return mid;
    else if (num < arr[mid]) end = mid - 1;
    else if (num > arr[mid]) start = mid + 1;
  }
  
  return -1;
}
```



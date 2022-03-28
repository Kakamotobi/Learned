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
  let low = 0;
  let high = arr.length - 1;
  let mid = 0;
  let idx = -1; // Initialize idx to -1 in case num does not exist in arr.
  
  while (low <= high) {
    mid = Math.floor((low + high) / 2);
    
    if (arr[mid] === num) {
      idx = mid; // Sync idx to an index containing num.
      if (isFirstOccurrence) high = mid - 1;
      else if (!isFirstOccurrence) low = mid + 1;
    } else if (arr[mid] < num) {
      low = mid + 1;
    } else {
      high = mid - 1;
    }
  }
  
  return idx;
}
```



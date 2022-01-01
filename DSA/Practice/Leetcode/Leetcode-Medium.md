# Leetcode - Medium

## 969. Pancake Sorting
- Input: array of integers.
- Output: an array of `k` values corresponding to a sequence of pancake flips that sort `arr`.
  - Any combintation where the array is sorted within `arr.length*10` flips.
### Example
```js
pancakeSort([3,2,4,1]) // [4,2,3,1] or [3,4,2,3,2], or etc.
pancakeSort([1,2,3]) // [] or [3,3], or etc.
```
### Solution
```js
const pancakeSort = (arr) => {
  // Initialize result.
  let result = [];

  // Initialize numbers sorted.
  let numsSorted = 0;

  // While all numbers haven't been sorted yet.
  while (numsSorted !== arr.length) {
    // Grab unsorted portion of arr.
    let unsortedPortion = arr.slice(0, arr.length-numsSorted);
    // Grab sorted portion of arr.
    let sortedPortion = arr.slice(arr.length-numsSorted, arr.length);

    // Find the maximum number among the non-sorted portion.
    let maxNum = Math.max(...unsortedPortion);

    // If maxNum is already at the end of unsortedPortion.
    if (unsortedPortion[unsortedPortion.length-1] === maxNum) {
      // Increment numsSorted.
      numsSorted++;
    }
    // Else if, maxNum is not at the end of unsortedPortion.
    else if (unsortedPortion[0] === maxNum) {
      // Flip from index 0 to the end of unsortedPortion.
      unsortedPortion = flip(unsortedPortion, unsortedPortion.length-1)

      // Update result.
      result.push(unsortedPortion.length);

      // Increment numsSorted.
      numsSorted++;
    }
    // Else, maxNum is in the middle of unsortedPortion.
    else {
      // Get index of maxNum.
      let maxNumIndex = unsortedPortion.indexOf(maxNum);

      // Flip from index 0 to the maxNum's index (effectively bringing maxNum to index 0).
      unsortedPortion = flip(unsortedPortion, maxNumIndex);

      // Update result.
      result.push(maxNumIndex+1);

      // Flip from index 0 to the end of unsortedPortion.
      unsortedPortion = flip(unsortedPortion, unsortedPortion.length-1);

      // Update result.
      result.push(unsortedPortion.length);

      // Increment numsSorted.
      numsSorted++;
    }
    // Update arr.
    arr = [...unsortedPortion, ...sortedPortion];
  }

  return result;
};

// Reverse numbers from index 0 to target index.
const flip = (arr, targetIndex) => {
  let pointer = targetIndex;

  for (let i = 0; i <= Math.floor(targetIndex/2); i++) {
    [arr[i],arr[pointer]] = [arr[pointer],arr[i]];
    pointer--;
  }

  return arr;
}
```

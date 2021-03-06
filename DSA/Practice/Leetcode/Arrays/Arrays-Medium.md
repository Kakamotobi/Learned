# Arrays - Medium

## 1010. Pairs of Songs With Total Durations Divisible by 60
- Input: array of integers representing song durations in seconds.
- Output: the number of pairs whose sum is divisible by 60.
  - `(time[i] + time[j]) % 60 === 0`
  - i.e., `(time[i] % 60) + (time[j] % 60) === 60`
### Example
```js
numPairsDivisibleBy60([30,20,150,100,40]) // 3
numPairsDivisibleBy60([60,60,60]) // 3
```
### Solution - Two Sum
```js
const numPairsDivisibleBy60 = (time) => {
  // Initialize counter.
  let counter = 0;
  
  // Initialize freq object.
  const freq = {};
  
  // Loop over time array.
  for (let t of time) {
    // Calculate mod of t.
    let mod = t % 60;
    // Calculate pair of t's mod.
    let otherModHalf;
    // Edge Case: mod === 0 refers to multiples of 60 (they need a pair that is a 0 or another multiple of 60). So, otherModHalf needs to be 0.
    if (mod === 0) otherModHalf = 0;
    else otherModHalf = 60 - mod;
    
    // If otherModHalf exists in freq, add the value of otherModHalf to counter. 
    if (freq[otherModHalf]) counter += freq[otherModHalf];
    
    // Record mod in freq.
    freq[mod] = ++freq[mod] || 1;
  }
  
  return counter;
}
```
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

## 347. Top K Frequent Elements
- Input: integer array `nums` and an integer `k`.
- Output: return the `k` most frequent elements (in any order).
### Example
```js
// Input: nums = [1,1,1,2,2,3], k = 2
// Output: [1,2]
```
### Solution - Bucket Sort
- Collect each number's frequency.
- Place each number and its frequency in an array (using its frequency as the index).
- Pop from the array k times.
```js
const topKFrequent = (nums, k) => {
  const freqMap = {};
  for (let num of nums) {
    freqMap[num] = ++freqMap[num] || 1;
  }
  
  const buckets = new Array(nums.length+1);
  for (let i = 0; i < buckets.length; i++) {
    buckets[i] = [];
  }
  
  for (let [num, freq] of Object.entries(freqMap)) {
    buckets[freq].push(num);
  }
  
  const ans = [];
  for (let i = buckets.length - 1; i > 0; i--) {
    for (let num of buckets[i]) {
      ans.push(num);
      if (ans.length === k) return ans;
    }
  }
}
```

## 238. Product of Array Except Self
- Input: integer array `nums`.
- Output: return an array `answer` such that `answer[i]` is equal to the product of all elements in `nums` except `nums[i]`.
- Constraints
  - `2 <= nums.length <= 105`
  - `-30 <= nums[i] <= 30`
  - The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
  - O(n) time without using the division operator.
### Solution 1 - TC: O(n), SC: O(n)
- Collect products to the left and right of each number.
- Then multiply the two.
```js
const productExceptSelf = (nums) => {
  const leftPortion = [];
  // Initialize with 1 for the first num since there is no number to the left of it.
  leftPortion[0] = 1;
  for (let i = 1; i < nums.length; i++) {
    leftPortion[i] = nums[i-1] * leftPortion[i-1];
  }
  
  const rightPortion = [];
  // Initialize with 1 for the last num since there is no number to the right of it.
  rightPortion[nums.length-1] = 1;
  for (let i = nums.length - 2; i >= 0; i--) {
    rightPortion[i] = nums[i+1] * rightPortion[i+1];
  }
  
  const answer = [];
  for (let i = 0; i < nums.length; i++) {
    answer[i] = leftPortion[i] * rightPortion[i];
  }
  
  return answer;
}
```
### Solution 2 - TC: O(n), SC: O(1)
- Optimize solution 1.
```js
const productExceptSelf = (nums) => {
  const answer = [];
  
  // Fill answer with left products first.
  // Initialize first number's left to 1 since there is no number to the left of it.
  answer[0] = 1;
  for (let i = 1; i < nums.length; i++) {
    answer[i] = nums[i-1] * answer[i-1];
  }
  
  // Then, update answer with right products using a variable to keep track of the accumulated product.
  let rightProd = 1;
  for (let i = nums.length - 1; i >= 0; i--) {
    answer[i] = answer[i] * rightProd;
    // Update rightProd for next iteration.
    rightProd = rightProd * nums[i];
  }
  
  return answer;
}
```

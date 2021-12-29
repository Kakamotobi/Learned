# Practice Problems

## Leetcode 1913. Maximum Product Difference Between Two Pairs
- Input: array of integers called nums.
- Output: largest product difference between pairs `(nums[w], nums[x])` and `(nums[y], nums[z])`.
### Solution 1
- TC: O(n log n)
- SC: O(1)
```js
const maxProductDifference = (nums) => {
  nums.sort((a,b) => a-b);

  const max1 = nums[nums.length-1];
  const max2 = nums[nums.length-2];
  const min1 = nums[0];
  const min2 = nums[1];

  return max1*max2 - min1*min2;
};
```
### Solution 2
- TC: O(n)
- SC: O(1)
```js
const maxProductDifference = (nums) => {
  // Initialize variables.
  let max = 0;
  let prevMax = 0;
  let min = 10 ** 4;
  let prevMin = 10 ** 4;

  // Loop over nums
  for (let num of nums) {
    // If num is greater than max.
    if (num > max) {
      // Set prevMax to max.
      prevMax = max;
      // Set max to num.
      max = num;
    }
    // If num is greater than prevMax (but less than max).
    else if (num > prevMax) {
      // Set prevMax to num.
      prevMax = num;
    }

    // If num is less than min.
    if (num < min) {
      // Set prevMin to min.
      prevMin = min;
      // Set min to num.
      min = num;
    }
    // If num is less than prevMin (but greater than min).
    else if (num < prevMin) {
      // Set prevMin to num
      prevMin = num;
    }
  }

  return max*prevMax - min*prevMin;
}
```

## Leetcode 1588. Sum of All Odd Length Subarrays
- Input: an array of positive integers.
- Output: the sum of all possible odd-length subarrays.
### Solution 1 - Sliding Window
- TC: O(n<sup>2</sup>)
- SC: O(1)
```js
const sumOddLengthSubarrays = function(arr) {
  // Initialize sum variable.
  let sum = 0;

  // Loop 1: (i simply representing odd length. ex: 1,3,5,7,9)
  for (let i = 1; i <= arr.length; i+=2) {
    // Initialize currSum.
    let currSum = 0;

    // Loop 2: (0 to current odd length) to add the first currSum of each of Loop 1's iteration. Necessary to initialize a first value to be able to implement a sliding window.
    for (let j = 0; j < i; j++) {
      // Calculate currSum.
      currSum += arr[j];
    }

    // Update sum with currSum.
    sum += currSum;

    // Loop 3: (odd Length to arr.length) to add up the rest of Loop 1's iteration.
    for (let j = i; j < arr.length; j++) {
      // Update currSum by subtracting first number and adding next number (sliding window).
      currSum = currSum - arr[j-i] + arr[j];
      // Update sum with currentSum;
      sum += currSum;
    }
  }

  return sum;
};
```


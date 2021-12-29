# Leetcode

## 1913. Maximum Product Difference Between Two Pairs
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

## 1588. Sum of All Odd Length Subarrays
- Input: an array of positive integers.
- Output: the sum of all possible odd-length subarrays.
### Solution - Sliding Window
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

## 88. Merge Sorted Array
- Input: two integer arrays `nums1` and `nums2`, each in non-decreasing order. Two integers `m` and `n` representing the number of elements in `nums1` and `nums2` respectively.
  - `nums1` is accommodated to have length of `m + n`. `m` elements are `nums1`'s numbers and `n` elements are set to `0`.
  - `nums2` has a length of `n`.
- Output: do not return the final sorted array. Store it inside the array `nums1`.
### Solution 1
- TC: O(n<sup>2</sup>)
- SC: O(1)
```js
const merge = function(nums1, m, nums2, n) {
  // Get rid of the 0s accounting for n in nums1.
  nums1.splice(nums1.length - n);

  // Initialize pointer j for nums2.
  let j = 0;

  // Loop over nums1.
  for (let i = 0; i < nums1.length; i++) {
    // If number in nums2 is less than current number in nums1.
    if (nums2[j] < nums1[i]) {
      // Insert the nums2 number in that position in nums1.
      nums1.splice(i, 0, nums2[j]);
      // Move on to next number in nums2.
      j++;
    }
  }

  // Loop over nums2.
  for (let k = j; k < nums2.length; k++) {
    // Push in any remaining numbers in nums2.
    nums1.push(nums2[k]);
  }
};
```
### Solution 2
- TC: O(n)
- SC: O(1)
```js
// Idea: start from the end and decrement indices.

// Version 1
const merge = (nums1, m, nums2, n) => {
  // Initialize three pointers to the last index of each corresponding array.
  let pointer1 = m - 1;
  let pointer2 = n - 1;
  let pointer3 = m + n - 1;

  // While pointer2 is a valid index for nums2.
  while (pointer2 >= 0) {
    // if number at nums1 is greater than number at nums2.
    if (nums1[pointer1] > nums2[pointer2]) {
      // Set the number at pointer3 of nums1 to be the number at pointer1 of nums1.
      nums1[pointer3] = nums1[pointer1]
      // Decrement pointer3 and pointer1.
      pointer3--;
      pointer1--;
    }
    // else (if number at nums1 is less than or equal to number at nums2).
    else {
      // Set the number at pointer3 of nums1 to be the number at pointer2 of nums2.
      nums1[pointer3] = nums2[pointer2]
      // Decrement pointer3 and pointer2.
      pointer3--;
      pointer2--;
    }
  }
}

// Version 2
const merge = (nums1, m, nums2, n) => {
  let pointer1 = m - 1;
  let pointer2 = n - 1;
  let pointer3 = m + n - 1;
  
  while (pointer2 >= 0) {
    nums1[pointer3--] = nums1[pointer1] > nums2[pointer2] ? nums1[pointer1--] : nums2[pointer2--];
  }
}
```






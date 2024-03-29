# Arrays - Easy

## Table of Contents
- [1. Two Sum](#1-two-sum)
- [121. Best Time to Buy and Sell Stock](#121-best-time-to-buy-and-sell-stock)
- [1913. Maximum Product Difference Between Two Pairs](#1913-maximum-product-difference-between-two-pairs)
- [1588. Sum of All Odd Length Subarrays](#1588-sum-of-all-odd-length-subarrays)
- [88. Merge Sorted Array](#88-merge-sorted-array)
- [278. First Bad Version](278-first-bad-version)
- [53. Maximum Subarray](#53-maximum-subarray)
- [1876. Substrings of Size Three with Distinct Characters](#1876-substrings-of-size-three-with-distinct-characters)
- [169. Majority Element](#169-majority-element)
- [26. Remove Duplicates from Sorted Array](#26-remove-duplicates-from-sorted-array)

## [1. Two Sum](https://leetcode.com/problems/two-sum/)
- Input: array of integer `nums`, and integer `target`.
- Output: return the indices of the two numbers that add up to target.
- Constraints
  - Each input has exactly one solution.
  - Cannot use the same element twice.
### Example
```js
// Input: nums = [2,7,11,15], target = 9
// Output: [0,1]
```
### Solution
- Use an object to store numbers and their indices.
```js
const twoSum = (nums, target) => {
  const obj = {};
  for (let i = 0; i < nums.length; i++) {
    const otherHalf = target - nums[i];
    if (otherHalf in obj) return [obj[otherHalf], i];
    else obj[nums[i]] = i;
  }
}
```

## [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- Input: array `prices` where `prices[i]` is the price of a given stock on the `i`th day.
- Output: return the maximum profit that can be achieved (buy low, sell high). Return `0` if no profit can be achieved.
### Example
```js
// Input: [7,1,5,3,6,4]
// Output: 5
```
### Solution
- Need a variable to keep track of the minimum price, which will be used to subtract from the future prices.
- Need a variable to keep track of the maximum profit.
```js
const maxProfit = (prices) => {
  let maxProfit = 0;
  let minPrice = Infinity;
  for (let price of prices) {
    // Update minPrice first.
    minPrice = Math.min(minPrice, price);
    // Update maxProfit if necessary.
    maxProfit = Math.max(maxProfit, price - minPrice);
  }
  return maxProfit;
}
```

## [1913. Maximum Product Difference Between Two Pairs](https://leetcode.com/problems/maximum-product-difference-between-two-pairs/)
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

## [1588. Sum of All Odd Length Subarrays](https://leetcode.com/problems/sum-of-all-odd-length-subarrays/)
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

## [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
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

## [278. First Bad Version](https://leetcode.com/problems/first-bad-version/)
- Input
  - Function called `isBadVersion` that determines whether a version is bad or not.
    - *Every version after the first bad version are bad.*
  - `n` versions.
- Output: a function to find the first bad version.
### Solution - Binary Search
- TC: O(log n)
- SC: O(1)
```js
const solution = function(isBadVersion) {
  return function(n) {
    // Initialize pointers.
    let start = 1;
    let end = n;
    let mid;
    
    // While start and end have not passed each other.
    while (start <= end) {
      // Update mid.
      mid = Math.floor((start+end)/2);
      // If mid is a bad version and mid - 1 is not a bad version, return mid (the first bad version).
      if (isBadVersion(mid) && !isBadVersion(mid-1)) return mid;
      // If mid is a bad version, update end.
      if (isBadVersion(mid)) end = mid - 1;
      // Else mid is not a bad version.
      else start = mid + 1;
    }
  }
}
```

## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- Input: integer array `nums`.
- Output: find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
- Constraints
  - `1 <= nums.length <= 105`
  - `-104 <= nums[i] <= 104`
### Solution - Sliding Window
- We know that negative prefixes should be dismissed.
  - Therefore, current sum should be reset to 0 if the current sum is negative up to `nums[i]`.
- Need a variable to keep track of the largest sum.
- Need a variable to keep track of the current sum through the iteration.
```js
const maxSubArray = (nums) => {
  // Initialize maxSum to the first number in nums.
  let maxSum = nums[0];
  // Initialize variable to keep track of current sum.
  let currSum = 0;
  
  for (let num of nums) {
    // If the prefix to num is negative, reset to 0.
    if (currSum < 0) currSum = 0;
    // Update currSum.
    currSum += num;
    // Update maxSum.
    maxSum = Math.max(maxSum, currSum);
  }
  
  return maxSum;
}
```

## [1876. Substrings of Size Three with Distinct Characters](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters/)
- Input: a string `s`.
- Output: return the number of substrings of length three in `s` that have no repeated characters.
- Constraints
  - `1 <= s.length <= 100`.
  - `s` consists of lowercase English letters.
### Expectation
```js
countGoodSubstrings("xyzzaz") // 1
countGoodSubstrings("aababcabc") // 4
```
### Solution - Sliding Window
- If current window does not include duplicates, increment the number of good substrings.
- Slide window.
```js
const countGoodSubstrings = (s) => {
  let numOfGoodSubstrings = 0;
  let windowStart = 0;
  for (let windowEnd = 3; windowEnd <= s.length; windowEnd++) {
    let currWindow = s.slice(windowStart, windowEnd);
    if (isGoodSubstring(currWindow)) numOfGoodSubstrings++;
    windowStart++;
  }
  return numOfGoodSubstrings;
}

const isGoodSubstring = (str) => {
  const freqMap = {};
  for (let char of str) {
    freqMap[char] = ++freqMap[char] || 1;
    if (freqMap[char] > 1) return false;
  }
  return true;
}
```

## [169. Majority Element](https://leetcode.com/problems/majority-element/)
### Solution 1
```js
// TC: O(n), SC: O(n)

const majorityElement = (nums) => {
  const maj = Math.ceil(nums.length / 2);
  
  const obj = {};
  
  for (let num of nums) {
    obj[num] = ++obj[num] || 1;
    if (obj[num] === maj) return num;
  }
};
```
### Solution 2 - Boyer-Moore Algorithm
- Use a variable to keep track of the number that is potentially the majority.
- Use another variable to keep track of that number's count.
- Switch to another number when the number's count becomes 0.
  - This means there were equal amounts of numbers until this point. So simply disregard previous numbers and start fresh.
- There is always a majority number. Therefore, "the last number standing" will be the majority number.
```js
// TC: O(n), SC: O(1)

const majorityElement = (nums) => {
  const maj = Math.ceil(nums.length / 2);

  // how do I accumulate data without using an auxiliary data structure?
  // There's always going to be only two different numbers.
  let temp = nums[0];
  let count = 0;

  for (let num of nums) {
    if (count === 0) {
      temp = num;
      count = 1;
    } else if (num === temp) {
      count++;
    } else {
      count--;
    }
  }

  return temp;
}
```

## [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
### Solution
```js
// TC: O(n), SC: O(1)

const removeDuplicates = (nums) => {
  let i = 0;
  let j = 0;
  
  while (i < nums.length - 1) {
    if (nums[j] === nums[i]) j++;
    else {
      nums.splice(i + 1, j - i - 1);
      i++;
      j = i + 1;
    }
  }
  
  return nums.length;
}
```

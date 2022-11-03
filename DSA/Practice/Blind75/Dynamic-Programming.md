# Dynamic Programming

## 1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
### Solution 1 - Brute Force (Pure Recursion)
```js
// TC: O(2^n), SC: O(n)

const climbStairs = (n) => {
  let numDistinctWays = 0;
  
  const helper = (num) => {
    if (num === n) {
      numDistinctWays++;
      return;
    }
    if (num > n) return;
    
    helper(num + 1);
    helper(num + 2);
  }
  
  helper(0);
  return numDistinctWays;
}
```
### Solution 2 - Dynamic Programming (Memoization)
```js
// TC: O(n), SC: O(n)

const climbStairs = (n) => {
  let numDistinctWays = 0;
  
  const memo = {};
  
  const helper = (num) => {
    if (num === n) {
      numDistinctWays++;
      return;
    }
    if (num > n) return;
    if (memo[num] !== undefined) {
      numDistinctWays += memo[num];
      return;
    }
    
    helper(num + 1);
    helper(num + 2);
    
    memo[num] = numDistinctWays;
  }
  
  helper(0);
  return numDistinctWays;
}
```
### Solution 3 - Dynamic Programming (Tabulation)
```js
// TC: O(n), SC: O(n)

const climbStairs = (n) => {
  const table = new Array(n);
  table[n] = 1;
  table[n-1] = 1;
  
  for (let i = n - 2; i > 0; i--) {
    table[i] = table[i+1] + table[i+2];
  }
  
  return table[0];
}
```
```js
// TC: O(n), SC: O(1)

const climbStairs = (n) => {
  if (n === 1) return 1;

  let numDistinctWays = 0;
  
  let one = 1;
  let two = 1;

  for (let i = 0; i < n; i++) {
    numDistinctWays = one + two;
    
    two = one;
    one = numDistinctWays;
  }
  
  return numDistinctWays;
}

// Alternative
const climbStairs = (n) => {
  let one = 1;
  let two = 1;
  let temp;

  for (let i = 0; i < n - 1; i++) {
    temp = one;
    one = one + two;
    two = temp;
  }

  return one;
}
```

## 2. [House Robber](https://leetcode.com/problems/house-robber/)
- Recurrence Relation
  - Rob the current house (`nums[i]`) and find the maximum from the remaining houses (`nums[i+2:nums.length]`).
  - Skip the current house and find the maximum from the remaining houses (`nums[i+1:nums.length]`).
- The subproblem is to find the maximum of the subarray of the entire array.
### Solution 1 - Recursion
```js
// TC: O(2^n)
// SC: O(n)

const rob = (nums) => {
  const helper = (i) => {
    if (i >= nums.length) return 0;
  
    return Math.max(nums[i] + helper(i+2), helper(i+1));
  }
  
  return helper(0);
}
```
### Solution 2 - Top-Down (Memoization)
```js
// TC: O(n)
// SC: O(n)

const rob = (nums) => {
  const memo = {};
  
  const helper = (i) => {
    if (i >= nums.length) return 0;
    if (memo[i] !== undefined) return memo[i];
    
    memo[i] = Math.max(nums[i] + helper(i+2), helper(i+1));
    
    return memo[i];
  }
  
  return helper(0);
}
```
### Solution 3 - Bottom-Up (Tabulation)
```js
// TC: O(n)
// SC: O(n)

const rob = (nums) => {
  // table[i] represents the maximum loot possible up to that point.
  const table = new Array(nums.length+1);
  table[0] = 0;
  table[1] = nums[0];
  
  for (let i = 1; i < nums.length; i++) {
    table[i+1] = Math.max(table[i], table[i-1] + nums[i]);
  }
  
  return table[nums.length];
}
```
### Solution 4 - Bottom-Up (2 Variables)
- From the Tabulation approach, we essentially only need to know the maximum possible loot for `i-1` and `i-2` in order to determine the maximum possible loot for `i`. Therefore, instead of a whole array, we can simply use two variables to keep track of the maximum possible loot for `i-1` and `i-2`.
```js
// TC: O(n)
// SC: O(1)

const rob = (nums) => {
  let maxUntilPrevHouse = 0;
  let maxUntilHouseBeforePrevHouse = 0;
  let temp;
  
  for (let num of nums) {
    temp = Math.max(num + maxUntilHouseBeforePrevHouse, maxUntilPrevHouse);
    maxUntilHouseBeforePrevHouse = maxUntilPrevHouse;
    maxUntilPrevHouse = temp;
  }
  
  return maxUntilPrevHouse;
}
```

## 3. [House Robber II](https://leetcode.com/problems/house-robber-ii/)
- Recurrence Relation
  - Same as [House Robber](#2-house-robber) but with one more constraint: the first and last houses are considered adjacent.
    - i.e. if the first house was robbed, you cannot rob the last house, and vice versa.
### Solution 1 - Recursion
- Calculate the max for two subarrays and return the max.
  - Subarray including the first but excluding the last house.
  - Subarray excluding the first but including the last house.
```js
// TC: O(2^n), SC: O(n)

const rob = (nums) => {
  if (nums.length === 1) return nums[0];

  const helper = (idx, arr) => {
    if (idx >= arr.length) return 0;
    
    return Math.max(arr[i] + helper(idx+2, arr), helper(idx+1, arr));
  }
  
  const maxInclFirstHouse = helper(0, nums.slice(0, nums.length - 1));
  const maxExclFirstHouse = helper(0, nums.slice(1));
  
  return Math.max(maxInclFirstHouse, maxExclFirstHouse);
}
```
### Solution 2 - Top-Down (Memoization)
```js
// TC: O(n), SC: O(n)

const rob = (nums) => {
  if (nums.length === 1) return nums[0];

  let memo = {};

  const helper = (idx, arr) => {
    if (idx >= arr.length) return 0;
    if (memo[idx] !== undefined) return memo[idx];

    memo[idx] = Math.max(arr[idx] + helper(idx+2, arr), helper(idx+1, arr));

    return memo[idx];
  }

  const maxInclFirstHouse = helper(0, nums.slice(0, nums.length - 1));
  memo = {}; // reset memo
  const maxExclFirstHouse = helper(0, nums.slice(1));

  return Math.max(maxInclFirstHouse, maxExclFirstHouse);
}
```
### Solution 3 - Bottom-Up (2 Variables)
```js
// TC: O(n), SC: O(1)

const rob = (nums) => {
  if (nums.length === 1) return nums[0];

  const helper = (arr) => {
    let maxUntilPrevHouse = 0;
    let maxUntilHouseBeforePrevHouse = 0;
    let temp;

    for (let num of arr) {
      temp = Math.max(num + maxUntilHouseBeforePrevHouse, maxUntilPrevHouse);
      maxUntilHouseBeforePrevHouse = maxUntilPrevHouse;
      maxUntilPrevHouse = temp;
    }

    return maxUntilPrevHouse;
  }

  const maxInclFirstHouse = helper(nums.slice(0, nums.length - 1));
  const maxExclFirstHouse = helper(nums.slice(1));

  return Math.max(maxInclFirstHouse, maxExclFirstHouse);
}
```

## 3. [Combination Sum](https://leetcode.com/problems/combination-sum/)
### Naive Approach
- At each point, create `candidates.length` number of recursive branches.
  - Ex: if `candidates` = `[2,3,6,7]` for combination `[2]`, we branch out to `2,3,6,7`, for combination`[2,3]` we branch out to `2,3,6,7`, and so on.
- This approach is successful in finding combinations but results to duplicates.
### Solution
- At each point, create 2 recursive branches.
    1) Get all combinations that henceforth, include this number at least once.
    2) Get all combinations that henceforth, do NOT include this number (no combination in this path will contain this number).
- Ex: the combination `[2,2]` branches out to `[2,2,2]`(where this number is kept added on) and `[2,2]` (where this number is no longer added on. i.e. move on to the next number).
```js
// TC: O(2^target), SC: O(target)

const combinationSum = (candidates, target) => {
  const result = [];
  
  const helper = (idx, combination, currSum) => {
    if (idx >= candidates.length) return;
    if (currSum > target) return;
    if (currSum === target) {
      result.push(combination);
      return;
    }
    
    // Keep including duplicates of this number.
    helper(idx, [...combination, candidates[idx]], currSum + candidates[idx]);
    // Don't add this number anymore and move on to the next number.
    helper(idx+1, combination, currSum);
  }
  
  helper(0, [], 0);
  
  return result;
}
```

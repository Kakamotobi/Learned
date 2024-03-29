# Dynamic Programming - Medium

## Table of Contents
- [198. House Robber](#198-house-robber)
- [213. House Robber II](#213-house-robber-ii)
- [1911. Maximum Alternating Subsequence](#1911-maximum-alternating-subsequence)
- [416. Partition Equal Subset Sum](#416-partition-equal-subset-sum)
- [494. Target Sum](#494-target-sum)
- [322. Coin Change (Unbounded Knapsack Problem)](#322-coin-change-unbounded-knapsack-problem)
- [518. Coin Change 2 (Unbounded Knapsack Problem)](#518-coin-change-2-unbounded-knapsack-problem)

## [198. House Robber](https://leetcode.com/problems/house-robber/description/)
- At each step, there are two branches: rob current house, or not rob current house.
- Recurrence Relation
  - *M(n)*: the maximum amount that can be robbed up to the `n`th house, 
  - *A(n)*: the amount that can be robbed at the `n`th house.
  - *M(n)* = Maximum between *A(n) + M(n-2)* and *M(n-1)*
### Solution 1 - Memoization
```js
// TC: O(n), SC: O(n)

const rob = (nums) => {
  const memo = {};
  
  const helper = (idx) => {
    if (idx >= nums.length) return 0;
    if (memo[idx] !== undefined) return memo[idx];
    
    memo[idx] = Math.max(nums[idx] + helper(idx+2), helper(idx+1));
    
    return memo[idx];
  }
  
  return helper(0);
}
```
### Solution 2 - Tabulation
- Can be optimized to `SC: O(1)` by using two variables instead of an array.
```js
// TC: O(n), SC: O(n)

const rob = (nums) => {
  const dp = [nums[0], Math.max(nums[0], nums[1])];
  
  for (let i = 2; i < nums.length; i++) {
    dp[i] = Math.max(nums[i] + dp[i-2], dp[i-1]);
  }
  
  return dp[nums.length - 1]; // `nums.length` instead of `dp.length` to account for when `nums = [0]`.
}
```

## [213. House Robber II](https://leetcode.com/problems/house-robber-ii/)
### Solution 1 - Brute Force
```js
// TC: O(2^n), SC: O(n)

const rob = (nums) => {
  const helper = (idx, arr) => {
    if (idx >= arr.length) return 0;
    
    return Math.max(nums[idx] + helper(idx+2, arr), helper(idx+1, arr));
  }
  
  return Math.max(helper(0, nums.slice(0, nums.length-1), helper(0, nums.slice(1)));
}
```
### Solution 2 - Memoization
```js
// TC: O(n), SC: O(n)

const rob = (nums) => {
  let memo = {};

  const helper = (idx, arr) => {
    if (idx >= arr.length) return 0;
    if (memo[idx] !== undefined) return memo[idx];

    memo[idx] = Math.max(arr[idx] + helper(idx+2, arr), helper(idx+1, arr));

    return memo[idx];
  }

  const maxInclFirstHouse = helper(0, nums.slice(0, nums.length-1));
  memo = {};
  const maxExclFirstHouse = helper(0, nums.slice(1));

  return Math.max(maxInclFirstHouse, maxExclFirstHouse);
};
```
### Solution 3 - Tabulation
- Can be optimized to `SC: O(1)` by using two variables instead of an array.
```js
// TC: O(n), SC: O(n)

const rob = (nums) => {
  if (nums.length === 1) return nums[0];
  
  const helper = (arr) => {
    const dp = [arr[0], Math.max(arr[0], arr[1])];
    
    for (let i = 2; i <= arr.length; i++) {
      dp[i] = Math.max(arr[i] + dp[i-2], dp[i-1]);
    }
    
    return dp[dp.length-1];
  }
  
  const maxInclFirstHouse = helper(nums.slice(0, nums.length-1));
  const maxExclFirstHouse = helper(nums.slice(1));
  
  return Math.max(maxInclFirstHouse, maxExclFirstHouse);
}
```

## [1911. Maximum Alternating Subsequence](https://leetcode.com/problems/maximum-alternating-subsequence-sum/description/)
- At each step, there are two branches: include this number, or don't include this number.
- If including the number, you need to know whether to add it or subtract it from the total.
### Solution 1 - Brute Force
```js
// TC: O(2^n), SC: O(n)

const maxAlternatingSum = (nums) => {
  let maxAltSum = -Infinity;

  const calcAlternatingSum = (arr) => {
    let evenSum = 0;
    let oddSum = 0;

    for (let i = 0; i < arr.length; i++) {
      if (i % 2 === 0) evenSum += arr[i];
      else oddSum += arr[i];
    }

    return evenSum - oddSum;
  }

  const helper = (idx, subsequence) => {
    if (idx === nums.length) {
      maxAltSum = Math.max(maxAltSum, calcAlternatingSum(subsequence));
      return;
    }

    helper(idx+1, subsequence); // Don't include this num.
    helper(idx+1, [...subsequence, nums[idx]]); // Include this num.
  }

  helper(0, []);

  return maxAltSum;
};
```
### Solution 2 - Memoization
- Use a variable to keep track of even-indexed numbers to be added and odd-indexed numbers to be subtracted, *in the (reindexed) subsequence*.
  - i.e. whether `nums[idx]` should be added or subtracted.
```js
// TC: O(n), SC: O(n)

const maxAlternatingSum = (nums) => {
  const memo = {};
  
  const helper = (idx, isEven) => {
    if (idx === nums.length) return ;
    if (memo[`${idx},${isEven}`] !== undefined) return memo[`${idx},${isEven}`];
    
    const num = isEven ? nums[idx] : -nums[idx];
    memo[`${idx},${isEven}`] = Math.max(num + helper(idx+1, !isEven), helper(idx+1, isEven));
    
    return memo[`${idx},${isEven}`];
  }
  
  return helper(0, true);
}
```

## [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
### Solution 1 - Brute Force
- Check all possible subsets to see if any is equal to the sum of all numbers in `nums` divided by 2.
```js
// TC: O(2^n), SC: O(n*t)
  // n: `nums.length`
  // t: the sum of all elements in `nums` / 2

const canPartition = (nums) => {
  const targetSum = nums.reduce((acc, curr) => acc + curr, 0) / 2;
  
  const helper = (idx, runningSum) => {
    if (runningSum === targetSum) return true;
    if (runningSum > targetSum) return false;
    if (idx === nums.length) return false;
    
    return helper(idx+1, runningSum) || helper(idx+1, runningSum + nums[idx]);
  }
  
  return helper(0, 0);
}
```
### Solution 2 - Memoization
```js
// TC: O(n*t), SC: O(t)

const canPartition = (nums) => {
  const targetSum = nums.reduce((acc, curr) => acc + curr, 0) / 2;

  if ((targetSum % 1) !== 0) return false; // If targetSum is a decimal.

  const memo = {};

  const helper = (idx, runningSum) => {
    if (runningSum === targetSum) return true;
    if (runningSum > targetSum) return false;
    if (idx === nums.length) return false;
    if (memo[`${idx},${runningSum}`] !== undefined) return memo[`${idx},${runningSum}`];

    memo[`${idx},${runningSum}`] = helper(idx+1, runningSum) || helper(idx+1, runningSum + nums[idx]);

    return memo[`${idx},${runningSum}`];
  }

  return helper(0, 0);
}
```
### Solution 3 - Bottom-Up
- Recurrence Relation
  - If a possible sum from the subset after the current number is the target sum, return true.
  - If the current number + a possible sum from the subset after the current number is the target sum, return true.
```js
// TC: O(n*t), SC: O(t)

const canPartition = (nums) => {
  const targetSum = nums.reduce((acc, curr) => acc + curr, 0) / 2;

  if ((targetSum % 1) !== 0) return false;

  // Accumulate all possible sums.
  let allPossibleSums = new Set([0]);
  for (let i = nums.length-1; i >= 0; i--) {
    // Create a temporary Set since we cannot modify an iterative (`allPossibleSums`) while iterating over it.
    const temp = new Set();

    // Accumulate all possible sums from nums[i] to the end.
    for (let sum of allPossibleSums) {
      temp.add(sum + nums[i]);
      temp.add(sum); // Need to include `sum` as well (avoid another loop to merge `temp` and `allPossibleSums`).
    }

    allPossibleSums = temp;
  }

  return allPossibleSums.has(targetSum);
}
```

## [494. Target Sum](https://leetcode.com/problems/target-sum/)
- Recurrence Relation
  - Each number can be added or subtracted.
### Solution 1 - Memoization
```js
// TC: O(n*t), SC: O(n)
  // n: `nums.length`
  // t: the number of values that can be passed in as `runningSum`
    // i.e. the extremes where all is positive or negative. Ex: if `nums = [1,2,3]`, `-6 <= t <= 6`.

const findTargetSumWays = (nums, target) => {
  const memo = {};

  const helper = (idx, runningSum) => {
    if (idx === nums.length && runningSum === target) return 1;
    if (idx === nums.length) return 0;
    if (memo[`${idx},${runningSum}`] !== undefined) return memo[`${idx},${runningSum}`];
    
    memo[`${idx},${runningSum}`] = helper(idx+1, runningSum + nums[idx]) + helper(idx+1, runningSum - nums[idx]);

    return memo[`${idx},${runningSum}`];
  }
  
  return helper(0, 0);
}
```

## [322. Coin Change (Unbounded Knapsack Problem)](https://leetcode.com/problems/coin-change/)
- Inputs:
  - An array of coins `coins` of different denominations.
  - An integer `amount`.
- Output: return the fewest number of coins that you need to make up `amount`. If that amount of money cannot be made up by any combination of coins, return `-1`.
- Description
  - Assume that you have an infinite number of each kind of coin.
- Constraints
- `1 <= coins.length <= 12`.
  - `1 <= coins[i] <= 231 - 1`.
  - `0 <= amount <= 104`.
### Example
```js
coinChange([1,2,5], 11); // 3
coinChange([2], 3); // -1
coinChange([1], 0); // 0
```
### Solutions
#### Solution 1 - Pure Recursion (brute force)
- **TC: O(n^t)**
  - n: the number of items in `coins`.
  - t: the number of ways to sum to `amount` using items in `coins`.
- Key Information
  - Each coin can be used as many times as needed.
- Keep track of:
  - The remaining sum.
- Base Cases
  - If the remaining sum is 0 (the path sums up to `amount`).
  - If the remaining sum is less than 0 (the path's sum exceeds `amount`).
```js
const coinChange = (coins, amount) => {
  const helper = (remainingSum) => {
    // Base Cases
    if (remainingSum === 0) return 0;
    if (remainingSum < 0) return Infinity;
    
    let minNumCoins = Infinity;
    
    for (let coin of coins) {
      minNumCoins = Math.min(minNumCoins, helper(remainingSum - coin));
    }
    
    return minNumCoins + 1;
  }
  
  const res = helper(amount);
  return res === Infinity ? -1 : res;
}
```
#### Solution 2 - Memoization
- Cache subproblems.
```js
const coinChange = (coins, amount) => {
  const memo = {};

  const helper = (remainingSum) => {
    // Base Cases
    if (memo[remainingSum] !== undefined) return memo[remainingSum];
    if (remainingSum === 0) return 0;
    if (remainingSum < 0) return Infinity;
    
    let minNumCoins = Infinity;
    
    for (let coin of coins) {
      minNumCoins = Math.min(minNumCoins, helper(remainingSum - coin));
    }
    
    memo[remainingSum] = minNumCoins + 1;
    
    return memo[remainingSum];
  }
  
  const res = helper(amount);
  return res === Infinity ? -1 : res;
}
```

## [518. Coin Change 2 (Unbounded Knapsack Problem)](https://leetcode.com/problems/coin-change-ii/)
- Inputs:
  - An array of coins `coins` of different denominations.
  - An integer `amount`.
- Output: return the number of combinations using the coins in `coins` to sum up to `amount`. Return 0 if there are none.
- Constraints
  - Each coin can be used infinitely.
  - `1 <= coins.length <= 300`.
  - `1 <= coins[i] <= 5000`.
  - All the values of coins are unique.
  - `0 <= amount <= 5000`.
### Examples
```js
change(5, [1,2,5]); // 4
change(3, [2]); // 0
change(10, [10]); // 1
```
### Solutions
#### Solution 1 - Pure Recursion (brute force)
- **TC: O(n^t)**
  - n: the number of items in `coins`.
  - t: the `amount` as it will decide the height of the decision tree.
- Since no duplicate combinations are allowed (Ex: `1+2+2` and `2+1+1` should be considered as one combination):
  - For the first coin, allow the use of all coins.
  - For the second coin, exclude the first coin that was used (thereby, no duplicate combinations will exist between the first and second coin).
  - For the third coin, exclude the first and second coin that was used (thereby, no duplicate combinations will exist between the first, second, and third coin).
- Keep track of:
  - The remaining sum.
  - The index of the current coin to exclude duplicate combinations.
- Base Cases
  - If `amount` is reached, return `1`.
  - If `amount` is exceeded, return `0`.
```js
const change = (amount, coins) => {
  const helper = (idx, remainingSum) => {
    // Base Cases
    if (remainingSum === 0) return 1;
    if (remainingSum < 1) return 0;
    
    let count = 0;
    for (let i = idx; i < coins.length; i++) {
      count += helper(i, remainingSum - coins[i]);
    }
    
    return count;
  }
  
  return helper(0, amount);
}
```
#### Solution 2 - Memoization
```js
const change = (amount, coins) => {
  const memo = {};

  const helper = (idx, remainingSum) => {
    // Base Cases
    if (remainingSum === 0) return 1;
    if (remainingSum < 1) return 0;
    if (memo[[idx, remainingSum]] !== undefined) return memo[[idx, remainingSum]];
    
    let count = 0;
    for (let i = idx; i < coins.length; i++) {
      count += helper(i, remainingSum - coins[i]);
    }
    
    memo[[idx, remainingSum]] = count;
    
    return memo[[idx, remainingSum]];
  }
  
  return helper(0, amount);
}
```

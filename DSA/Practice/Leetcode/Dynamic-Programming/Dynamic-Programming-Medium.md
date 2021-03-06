# Dynamic Programming - Medium

## 494. Target Sum (0/1 Knapsack Problem)
- Inputs
  - An array of integers `nums`.
  - An integer `target`.
- Output: return the number of different expressions that can be built with all the integers in `num`, which evaluate to `target`.
- Description
  - An expression refers to adding `+` or `-` before each integer in `nums`, and then concatenate all the integers.
### Example
```js
findTargetSumWays([1,1,1,1,1], 3); // 5
  // -1 + 1 + 1 + 1 + 1 = 3
  // +1 - 1 + 1 + 1 + 1 = 3
  // +1 + 1 - 1 + 1 + 1 = 3
  // +1 + 1 + 1 - 1 + 1 = 3
  // +1 + 1 + 1 + 1 - 1 = 3
```
### Solution 1 - Pure Recursion (brute force)
- **TC: O(2<sup>n</sup>)**
- Keep track of:
  - The index (representing the current number in `nums` that is in consideration).
  - The total sum so far.
- Base Case
  - If index is out of bounds (we have reached the end of `nums`).
- At each call/step we can make one of two choices (`+` or `-`).
```js
const findTargetSumWays = (nums, target) => {
  // helper eventually returns the number of successful expressions.
  const helper = (idx, total) => {
    // Base Case
    if (idx >= nums.length) {
     return total === target ? 1 : 0;
    }
    
    // helper returns either 1 (expression is successful) or 0 (expression is unsuccessful), representing the number of successful expressions.
    // Therefore, once the recursive calls backtrack to the first call, the number of successful expressions is computed.
    return helper(idx + 1, total + nums[idx]) + helper(idx + 1, target - nums[idx]);
  }
  
  return helper(0,0);
}
```

### Solution 2 - Memoization
- **TC: O(n\*t)**
  - n: the number of items in `nums`.
  - t: the total possible values of `target`. i.e., the sum of the entire array `nums` (Ex: if `nums = [1,2,3]`, `-6 <= t <= 6`).
- Additional Base Case
  - If the `idx`, `total` pair was seen before, and thus exists in our cache.
```js
const findTargetSumWays = (nums, target) => {
  const memo = {};
  
  const helper = (idx, total) => {
    // Base Cases
    if (idx >= nums.length) {
      return total === target ? 1 : 0;
    }
    if (memo[[idx, total]] !== undefined) return memo[[idx, total]];
    
    // Cache the result.
    memo[[idx, total]] = helper(idx + 1, total + nums[idx]) + helper(idx + 1, total - nums[idx]]);
    
    return memo[[idx, total]];
  }
  
  return helper(0,0);
}
```

## 416. Partition Equal Subset Sum (0/1 Knapsack Problem)
- Input: a non-empty array of only positive integers `nums`.
- Output: return true if `nums` can be partitioned into two subsets such that the sum of elements in both subsets is equal.
### Example
```js
canPartition([1,5,11,5]); // true because [1,5,5] and [11].
canPartition([1,2,3,5]); // false.
```
### Solution 1 - Pure Recursion (brute force)
- **TC: O(2<sup>n</sup>)**
- "... the sum of both subsets is equal" means that the function should return `true` if any single subset is equal to the sum of `nums` divided by 2.
  - Therefore, we have to essentially determine every single sum that can be made with any single subset from `nums`. And check if any of those sums equal the sum of nums / 2.
  - At any point, if we have a running total of the sum of all elements in the array divided by 2, the solution should return `true`.
- Keep track of:
  - The index (representing the current number in `nums` that is in consideration).
  - The total sum so far.
- Base Cases
  - If index is out of bounds, return false (we have reached the end of `nums` but was not able to sum to target).
  - If the current total is greater than the target, return false (we don't need to continue down the path).
  - If the current total is equal to the target, return true.
- At each call/step we can make one of two choices (use the number at the current index or not).
```js
const canPartition = (nums) => {
  let target = 0;
  for (let num of nums) {
    target += num;
  }
  // Edge Case
  if (target % 2 !== 0) return false;
  else target /= 2;
  
  const helper = (idx, total) => {
    // Base Cases
    if (idx >= nums.length) return false;
    if (total > target) return false;
    if (total === target) return true;
    
    return helper(idx + 1, total + nums[idx]) || helper(idx + 1, total);
  }
  
  return helper(0,0);
}
```
### Solution 2 - Memoization
- **TC: O(n\*t)**
  - n: the number of items in `nums`.
  - t: the sum of all elements in `nums` / 2. Ex: if `nums = [1,2,3]`, `t = 6`.
- Keep track of:
  - The index (representing the current number in `nums` that is in consideration).
  - The total sum so far.
- Additional Base Case
  - If the `idx`, `total` pair was seen before, and thus exists in our cache.
```js
const canPartition = (nums) => {
  let target = 0;
  for (let num of nums) {
    target += num;
  }
  // Edge Case
  if (target % 2 !== 0) return false;
  else target /= 2;
  
  const memo = {};
  
  const helper = (idx, total) => {
    // Base Cases
    if (idx >= nums.length) return false;
    if (total > target) return false;
    if (total === target) return true;
    if (memo[[idx, total]] !== undefined) return memo[[idx, total]];
    
    memo[[idx, total]] = helper(idx + 1, total + nums[idx]) || helper(idx + 1, total);
    
    return memo[[idx, total]];
  }
  
  return helper(0,0);
}
```
### Solution 3 - Tabulation
- **TC: O(n\*t)**
  - n: the number of items in `nums`.
  - t: the sum of all elements in `nums` / 2. Ex: if `nums = [1,2,3]`, `t = 6`.
- Bottom-Up Approach.
- Store all the possible sums of any given subset from the remainder of the array.
  - For each possible sum in that subset:
    - If the sum is equal to the target, return true.
    - If the current number + the sum is equal to the target, return true.
- How many possible sums could we create from each subset?
- **Flow**
  ```js
  nums = [1,5,11,5];
  target = 11;

  // Possible sums using or not using [5]: [0,5].
  // Possible sums using or not using [11] after using or not using 5: [0,5,11,16,0]. But we don't need duplicates so, [0,5,11,16].
  // Possible sums using or not using [5] after using or not using 5 and 11: [0,5,11,16,5,10,16,21,0]. But we don't need duplicates so, [0,5,11,16,10,21].
  // Possible sums using or not using [1] after using or not using 5, 11, and 5: [0,5,11,16,10,21,1,6,12,17,11,22,0]. But we don't need duplicates so, [0,5,11,16,10,21,1,6,12,17,22].

  // [0,5,11,16,10,21,1,6,12,17,22] is the entire list of all the possible sums that can be computed from the given input nums.
  // If this list contains the target, return true. Otherwise, return false.
  ```
```js
const canPartition = (nums) => {
  let target = 0;
  for (let num of nums) {
    target += num;
  }
  // Edge Case
  if (target % 2 !== 0) return false;
  else target /= 2;
  
  const allPossibleSums = new Set([0]);
  for (let num of nums) {
    // Create a temporary Set since we cannot modify an iterative while iterating over it.
    const temp = new Set();
    for (let s of allPossibleSums) {
      temp.add(s + num);
    }
    // Join temp to allPossibleSums.
    for (let s of temp) {
      allPossibleSums.add(s);
    }
  }
  
  return allPossibleSums.has(target);
}
```

## 322. Coin Change (Unbounded Knapsack Problem)
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

## 518. Coin Change 2 (Unbounded Knapsack Problem)
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

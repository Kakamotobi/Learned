# Dynamic Programming - Medium

## 494. Target Sum
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

### Solution 2 - 0/1 Knapsack (Memoization)
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

## 416. Partition Equal Subset Sum
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
  - If index is out of bounds (we have reached the end of `nums`).
  - If the current total is equal to the target.
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
    if (total === target) return false;
    
    return helper(idx + 1, total + nums[idx]) || helper(idx + 1, total);
  }
  
  return helper(0,0);
}
```
### Solution 2 - 0/1 Knapsack (Memoization)
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
    if (total === target) return true;
    if (memo[[idx, total]] !== undefined) return memo[[idx, total]];
    
    memo[[idx, total]] = helper(idx + 1, total + nums[idx]) || helper(idx + 1, total);
    
    return memo[[idx, total]];
  }
  
  return helper(0,0);
}
```
### Solution 3 - 0/1 Knapsack (Tabulation)
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

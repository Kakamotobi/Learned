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
### Solution 1 - Pure Recursion
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

### Solution 2 - Dynamic Programming (Memoization)
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


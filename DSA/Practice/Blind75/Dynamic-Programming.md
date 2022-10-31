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
### Solution 2 - Memoization
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

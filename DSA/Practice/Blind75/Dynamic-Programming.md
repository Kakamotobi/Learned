# Dynamic Programming

## Table of Contents
- [1. Climbing Stairs](#1-climbing-stairs)
- [2. House Robber](#2-house-robber)
- [3. House Robber II](#3-house-robber-ii)
- [4. Combination Sum](#4-combination-sum)
- [5. Coin Change](#5-coin-change)
- [6. Longest Increasing Subsequence](#6-longest-increasing-subsequence)
- [7. Longest Common Subsequence](#7-longest-common-subsequence)
- [8. Word Break](#8-word-break)
- [9. Unique Paths](#9-unique-paths)

## 1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
### Solution 1 - Brute Force (Recursion)
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
### Solution 2 - Top-Down (Memoization)
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
### Solution 3 - Bottom-Up (Tabulation)
```js
// TC: O(n), SC: O(n)

// Forwards
const climbStairs = (n) => {
  const table = [1,1];
  
  for (let i = 2; i <= n; i++) {
    table[i] = table[i-1] + table[i-2];
  }
  
  return table[n];
}

// Backwards
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
### Solution 1 - Brute Force (Recursion)
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

// Approach 1
const rob = (nums) => {
  // table[i] represents the maximum loot possible up to that point.
  const table = [nums[0], Math.max(nums[0], nums[1])];
  
  for (let i = 2; i < nums.length; i++) {
    table[i] = Math.max(nums[i] + table[i-2], table[i-1]);
  }
  
  return table[nums.length - 1];
}

// Approach 2
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

// Approach 1
const rob = (nums) => {
  if (nums.length === 1) return nums[0];

  let maxUntilPrevHouse = Math.max(nums[0], nums[1]);
  let maxUntilPrevPrevHouse = nums[0];
  let temp;
  
  for (let i = 2; i < nums.length; i++) {
    temp = maxUntilPrevHouse;
    maxUntilPrevHouse = Math.max(nums[i] + maxUntilPrevPrevHouse, maxUntilPrevHouse);
    maxUntilPrevPrevHouse = temp;
  }
  
  return maxUntilPrevHouse;
}

// Approach 2
const rob = (nums) => {
  let maxUntilPrevHouse = 0;
  let maxUntilPrevPrevHouse = 0;
  let temp;
  
  for (let num of nums) {
    temp = Math.max(num + maxUntilPrevPrevHouse, maxUntilPrevHouse);
    maxUntilPrevPrevHouse = maxUntilPrevHouse;
    maxUntilPrevHouse = temp;
  }
  
  return maxUntilPrevHouse;
}
```

## 3. [House Robber II](https://leetcode.com/problems/house-robber-ii/)
- Recurrence Relation
  - Same as [House Robber](#2-house-robber) but with one more constraint: the first and last houses are considered adjacent.
    - i.e. if the first house was robbed, you cannot rob the last house, and vice versa.
### Solution 1 - Brute Force (Recursion)
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

## 4. [Combination Sum](https://leetcode.com/problems/combination-sum/)
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

## 5. [Coin Change](https://leetcode.com/problems/coin-change/)
### Solution 1 - Top-Down (Memoization)
- Each "step" has `coins.length` amount of branches.
- Helper function returns the fewest number of coins needed.
```js
// TC: O(n * m), SC: O(n * m)

const coinChange = (coins, amount) => {
  const memo = {};

  const helper = (runningSum) => {
    if (runningSum === amount) return 0;
    if (runningSum > amount) return Infinity;
    if (memo[runningSum]) return memo[runningSum];
    
    let fewestNumCoinsUsed = Infinity;
    for (let coin of coins) {
      fewestNumCoinsUsed = Math.min(fewestNumCoinsUsed, helper(runningSum + coin) + 1);
    }
    
    memo[runningSum] = fewestNumCoinsUsed;
    
    return fewestNumCoinsUsed;
  }
  
  const ans = helper(0);
  return ans === Infinity ? -1 : ans;
}
```
### Solution 2 - Bottom-Up (Tabulation)
- `table[i]` represents the fewest number of coins needed to sum to `i`.
  - `0 <= i <= amount`.
- First, check all coins for each `i` and store the minimum number of coins required.
- Example
  ```js
  // coins = [1,3,4,5], amount = 7
  
  // dp[0] = 0
  // dp[1] = 1 (coin 1) + dp[0] = 1
  // dp[2] = 1 (coin 1) + dp[1] = 2
  // dp[3] = 1 (coin 1) + dp[2] = 3
  //       = 1 (coin 3) + dp[0] = 1 <-- stored
  // dp[4] = 1 (coin 1) + dp[3] = 2
  //       = 1 (coin 3) + dp[1] = 2
  //       = 1 (coin 4) + dp[0] = 1 <-- stored
  // dp[5] = 1 (coin 1) + dp[4] = 2
  //       = 1 (coin 3) + dp[2] = 3
  //       = 1 (coin 4) + dp[1] = 2
  //       = 1 (coin 5) + dp[0] = 1 <-- stored
  // dp[6] = 1 (coin 1) + dp[5] = 2 <-- stored
  //       = 1 (coin 3) + dp[3] = 2
  //       = 1 (coin 4) + dp[2] = 3
  //       = 1 (coin 5) + dp[1] = 2
  // dp[7] = 1 (coin 1) + dp[6] = 3
  //       = 1 (coin 3) + dp[4] = 2 <-- stored
  //       = 1 (coin 4) + dp[3] = 2
  //       = 1 (coin 5) + dp[2] = 3
  ```
```js
// TC: O(m * n), SC: O(m)
  // m - `amount`
  // n - `coins.length`

const coinChange = (coins, amount) => {
  const table = [0];
  
  for (let i = 1; i <= amount; i++) {
    let minNumCoinsForI = Infinity;
    for (let coin of coins) {
      const remSum = i - coin;
      if (remSum < 0) continue;
      
      minNumCoinsForI = Math.min(minNumCoinsForI, 1 + table[remSum]);
    }
    table[i] = minNumCoinsForI;
  }
  
  return table[table.length - 1] === Infinity ? -1 : table[table.length - 1];
}
```
```js
// Alternative

const coinChange = (coins, amount) => {
  const table = new Array(amount + 1).fill(Infinity);
  table[0] = 0;
  
  for (let i = 1; i <= amount; i++) {
    for (let coin of coins) {
      const remSum = i - coin;
      if (remSum >= 0) {
        table[i] = Math.min(table[i], 1 + table[remSum]);
      }
    }
  }
  
  return table[table.length - 1] === Infinity ? -1 : table[table.length - 1];
}
```

## 6. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
- Numbers in the subsequence do not have to be contiguous.
### Solution 1 - Brute Force (Recursion)
- Generate every possible (meaning, strictly increasing subsequence) subsequence to find the longest one.
- Recursive Relation
  - Include or Not Include the current value to the current subsequence.
- Recursive function should return the length of the resulting longest subsequence when starting from that value.
- Base Cases
  - If `idx` is out of bounds.
  - If the current value is equal to or less than the last value considered in the current subsequence, cannot go further.
```js
// TC: O(2^n), SC: O(n)

const lengthOfLIS = (nums) => {
  const helper = (idx, lastValInSubsequence) => {
    if (idx >= nums.length) return 0;
    if (nums[idx] <= lastValInSubsequence) return helper(idx+1, lastValInSubsequence);
    
    // Return the length of the longest increasing subsequence derived from either including or not including the current value.
    return Math.max(1 + helper(idx+1, nums[idx]), helper(idx+1, lastValInSubsequence));
  }
  
  return helper(0, -Infinity);
}
```
```js
// Alternative

const lengthOfLIS = (nums) => {
  const helper = (idx, lastValInSubsequence) => {
    if (idx >= nums.length) return 0;
    if (nums[idx] <= lastValInSubsequence) return helper(idx+1, lastValInSubsequence);
    
    let ans = -Infinity;
    for (let i = idx; i < nums.length; i++) {
      if (nums[i] > lastValInSubsequence) {
        ans = Math.max(1+helper(idx+1, nums[idx]), helper(idx+1, lastValInSubsequence));
      }
    }
    return ans;
  }
  
  return helper(0, -Infinity);
}
```
### Solution 2 - Top-Down (Memoization)
#### Approach 1
- `memo[idx]` represents the length of the longest increasing subsequence when starting from the index `idx` in `nums`.
```js
// TC: O(n^2), SC: O(n)
  // n - nums.length

const lengthOfLIS = (nums) => {
  const memo = new Array(nums.length).fill(-Infinity);

  const helper = (idx, lastVal) => {
    if (idx >= nums.length) return 0;

    let left = 0;
    let right = 0;

    // Include this value.
    if (nums[idx] > lastVal) {
      if (memo[idx] !== -Infinity) {
        left = memo[idx];
      } else {
        left = 1 + helper(idx+1, nums[idx]);
        memo[idx] = left;
      }
    }

    // Don't include this value.
    right = helper(idx+1, lastVal);

    return Math.max(left, right);
  }

  return helper(0, -Infinity);
};
```
#### Approach 2
- Start from the end of `nums` and check every position that comes after the current number.
```js
// TC: O(n^2), SC: O(n)

const lengthOfLIS = (nums) => {
  // `memo[i]` represents the length of the longest strictly increasing subsequence starting from and INCLUDING index `i` to the end.
  const memo = new Array(nums.length).fill(1);
  
  for (let i = nums.length - 1; i >= 0; i--) {
    for (let j = i+1; j < nums.length; j++) {
      if (nums[i] < nums[j]) {
        // `1` represents the current number (`nums[i]`) by itself as the subsequence.
        // `memo[j]` represents the length of the LIS when starting from index `j`, which represents the LIS in `nums.slice(j)`.
        memo[i] = Math.max(memo[i], 1 + memo[j]);
      }
    }
  }
  
  return Math.max(...memo);
}
```

## 7. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
- Subsequences do not have to be contiguous.
- Main Problem: find the longest common subsequence from the two given strings.
- Sub Problem: find the longest common subsequence between the remainder of both strings.
  - Ex: "abcde" and "ace".
    - Since both have "a" in common, the problem becomes `1 + sub problem(longest common subsequence of the remainder of both strings)`.
    - If the characters are different, 2 sub problems arise (return the max of the two).
      - Check `text1` (incl. its char) and `text2` (excl. its char).
      - Check `text1` (excl. its char) and `text2` (incl. its char).
### Solution 1 - Brute Force (Recursion)
- Time Limit Exceeded
```js
// TC: O(n^2), SC: O()

const longestCommonSubsequence = (text1, text2) => {
  const helper = (t1Idx, t2Idx) => {
    if (t1Idx >= text1.length || t2Idx >= text2.length) return 0;
    
    if (text1[t1Idx] === text2[t2Idx]) return 1 + helper(t1Idx+1, t2Idx+1);
    else return Math.max(helper(t1Idx, t2Idx+1), helper(t1Idx+1, t2Idx));
  }
  
  return helper(0, 0);
}
```
### Solution 2 - Bottom-Up
- Use a 2D matrix where `matrix[i][j]` represents the longest common subsequence when starting from index `i` in `text1` and index `j` in `text2`.
- Start filling the matrix from the bottom/end.
```js
// TC: O(m*n), SC: O(m*n)

const longestCommonSubsequence = (text1, text2) => {
  const dp = new Array(text1.length + 1);
  for (let i = 0; i < text1.length + 1; i++) {
    dp[i] = new Array(text2.length + 1).fill(0);
  }
  
  for (let i = text1.length - 1; i >= 0; i--) {
    for (let j = text2.length - 1; j >= 0; j--) {
      if (text1[i] === text2[j]) dp[i][j] = 1 + dp[i+1][j+1];
      else dp[i][j] = Math.max(dp[i][j+1], dp[i+1][j]);
    }
  }
  
  return dp[0][0];
}
```

## 8. [Word Break](https://leetcode.com/problems/word-break/description/)
### Solution 1 - Top-Down (Memoization)
- Take each word in `wordDict`, and check if the corresponding substring of that length in `s` matches it.
- If found, start from the next position in `s`.
- Decision Tree
  - Branches = number of words in `wordDict`.
- Cache
  - Possible position in `s` that may yield false.
```js
// TC: O(n*m*n), SC: O(n)

const wordBreak = function(s, wordDict) {
  const memo = {};
  
  const helper = (idx) => {
    if (idx >= s.length) return true;
    if (memo[idx]) return memo[idx];
    
    let isBreakable = false;
    for (let word of wordDict) {
      const substring = s.substring(idx, idx + word.length);
      if (substring === word) {
        isBreakable = isBreakable || helper(idx + word.length);
      }
      
      if (isBreakable === true) break;
    }
    
    memo[idx] = isBreakable;
    
    return isBreakable;
  }
  
  return helper(0);
}
```
### Solution 2 - Bottom-Up (Tabulation)
- Start from the end of `s`.
- Is the substring (starting from index `i` to the length of each word in `wordDict`) word breakable?
```js
// TC: O(m*n), OC: O(n)
  // m - wordDict.length
  // n - s.length

const wordBreak = function(s, wordDict) {
  // + 1 for base case
  const dp = new Array(s.length + 1).fill(false);
  dp[s.length] = true; // the out of bounds position (base case) is always true.
  
  for (let i = s.length - 1; i >= 0; i--) {
    for (let word of wordDict) {
      // If the word is within bounds of `s` AND the substring is the same as word.
      if ((i + word.length) <= s.length &&
          s.slice(i, i + word.length) === word
      ) {
        // Since the above condition confirms "true" for this substring, check whether the remaining segment is true or not.
        dp[i] = dp[i + word.length];
      }
      
      // If found one that yields true, there is no need to check other words.
      if (dp[i] === true) break;
    }
  }
  
  return dp[0];
}
```

## 9. Unique Paths
### Solution 1 - Recursion (DFS)
- Time limit exceeded
```js
// TC: O(2^(m*n)), SC: O(m*n)

const uniquePaths = (m, n) => {
  const moves = [[1,0],[0,1]];
  
  const dfs = (x,y) => {
    if (x >= n || y >= n) return 0;
    if (x === m - 1 && y === n - 1) return 1;
    
    let numUniquePathsForThisPosition = 0;
    for (let [ax, ay] of moves) {
      numUniquePathsForThisPosition += dfs(x + ax, y + ay);
    }
    
    return numUniquePathsForThisPosition;
  }
  
  return dfs(0,0);
}
```
### Solution 2 - Top-Down (Memoization)
```js
// TC: O(m*n), SC: O(m*n)

const uniquePaths = (m, n) => {
  const moves = [[1,0],[0,1]];
  
  const memo = {};
  
  const dfs = (x,y) => {
    if (x >= n || y >= n) return 0;
    if (x === m - 1 && y === n - 1) return 1;
    if (memo[`${x},${y}`]) return memo[`${x},${y}`];
    
    let numUniquePathsForThisPosition = 0;
    for (let [ax, ay] of moves) {
      numUniquePathsForThisPosition += dfs(x + ax, y + ay);
    }
    
    memo[`${x},${y}`] = numUniquePathsForThisPosition;
    
    return numUniquePathsForThisPosition;
  }
  
  return dfs(0,0);
}
```
### Solution 3 - Bottom-Up (Tabulation)
- Subproblem: `grid[i][j] = grid[i+1][j] + grid[i][j+1]`.
  - `grid[i][j]` represents the number of possible unique paths from that position.
```js
// TC: O(m*n), SC: O(m*n)

const uniquePaths = (m, n) => {
  const grid = new Array(m+1);
  for (let i = 0; i < m+1; i++) {
    grid[i] = new Array(n+1).fill(0);
  }
  grid[m-1][n-1] = 1;
  
  for (let i = m-1; i >= 0; i--) {
    for (let j = n-1; j >= 0; j--) {
      // Skip the bottom-right corner
      if (i === m-1 && j === n-1) continue;
      // Below cell + Right cell
      grid[i][j] = grid[i+1][j] + grid[i][j+1];
    }
  }
  
  return grid[0][0];
}
```
### Solution 4 - Bottom-Up (One Array)
- The bottom-most row and right-most column are all `1`.
```js
// TC: O(m*n), SC: O(n)

const uniquePaths = (m, n) => {
  let prevRow = new Array(n).fill(1);

  for (let i = m-2; i >= 0; i--) {
    const newRow = new Array(n).fill(1);

    for (let j = n-2; j >= 0; j--) {
      newRow[j] = prevRow[j] + newRow[j+1];
    }

    prevRow = newRow;
  }

  return prevRow[0];
}
```

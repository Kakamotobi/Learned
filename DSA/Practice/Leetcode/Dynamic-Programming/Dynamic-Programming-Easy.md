# Dynamic Programming Easy

## [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- At each step, there are two branches: 1 step or 2 steps.
- Recurrence Relation
  - The number of distinct ways for the current step can be calculated by adding the number of distinct ways for the previous step and that of the one before previous.
  - `solution(3) = solution(2) + solution(1)` <-- Fibonacci
```js
// TC: O(n), SC: O(1)

const climbStairs = (n) => {
  if (n === 1) return 1;
  
  let curr = 0;
  let prev = 1;
  let prevPrev = 1;
  
  for (let i = 1; i <= n - 1; i++) {
    curr = prev + prevPrev;
    
    prevPrev = prev;
    prev = curr;
  }
  
  return curr;
}
```

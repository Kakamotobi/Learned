# Dynamic Programming Easy

## [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- At each step, there are two branches: 1 step or 2 steps.
- Recurrence Relation
  - For a staircase size of `N`, the first step is always `1` or `2`. Therefore, if the first step was `1` step, there are `N-1` steps left. Similarly, if the first step was `2` steps, the subproblem becomes `N-2`.
  - Therefore, `solution(N) = solution(N-1) + solution(N-2)` <-- Fibonacci
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

# Dynamic Programming

## Table of Contents
- [What is Dynamic Progamming?](#what-is-dynamic-programming)
- [How to Identify Dynamic Programming Problems](#how-to-identify-dynamic-programming-problems)
  - [Steps](#steps)
  - [Conditions](#conditions)
    - [1. Overlapping Subproblems](#1-overlapping-subproblems)
    - [2. Optimal Substructure](#2-optimal-substructure)
- [Approach for Solving DP Problems](#approach-for-solving-dp-problems)
- [Dynamic Programming Big O](dynamic-programming-big-o)
- [Example - Fibonacci Sequence](#example---fibonacci-sequence)
  - [Recursion Solution](#recursion-solution)
  - [Top-Down Dynamic Programming (Memoization) Solution](#top-down-dynamic-programming-memoization-solution)
  - [Bottom-Up Dynamic Programming (Tabulation) Solution](#bottom-up-dynamic-programming-tabulation-solution)
    - [2 Ways to Calculate Subproblems for Bottom-Up Approach](#2-ways-to-calculate-subproblems-for-bottom-up-approach)
      - [Bottom-Up Forward Dynamic Programming](#bottom-up-forward-dynamic-programming)
      - [Bottom-Up Backward Dynamic Programming](#bottom-up-backward-dynamic-programming)
- [Other Dynamic Programming Patterns](#other-dynamic-programming-patterns)
  - [0/1 Knapsack](#01-knapsack)
  - [Unbounded Knapsack](#unbounded-knapsack)

## What is Dynamic Programming?
> Dynamic Programming is mainly an optimization over plain recursion. Wherever we see a recursive solution that has repeated calls for same inputs, we can optimize it using Dynamic Programming. | GeeksforGeeks
- **A method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions so that we can reuse them.**
  - i.e. Using past knowledge to make solving a future problem easier.
- It is an approach that is applicable to a small subset of problems (most problems cannot be solved with it).
  - Kind of like "divide and conquer" in that it can make a huge difference for those problems that can be solved with it.

## How to Identify Dynamic Programming Problems
### Steps
1. **Can the problem solution be expressed as a solution to similar smaller problems**?
    - i.e. **does a recursive structure (recurrence relation) exist between subproblems?**
    - Ex: given an index, the last value in a subsequence, and the `nums` array, we could determine valid/possible options for a strictly increasing subsequence starting from that index. Furthermore, we can either include or not include the value at the index, which makes the problems smaller as we move up the index.
    - **This process should be repeatable until an obvious end.**
      - Ex: an obvious end would be the index going out of bounds, or the value being equal to or less than the last value in the subsequence.
2. **Express the problem in terms of the given parameters, and see which of those parameters are changing. Also, identify the number of subproblems.**
    - List examples of several subproblems and compare the parameters.
    - Ex: `solution(idx, lastValInSubsequence)` --> `solution(idx+1, nums[idx])` and `solution(idx+1, lastValInSubsequence)`.
      - There are two parameters that change.
3. **Clearly express the Recurrence Relation.**
    - Now that subproblems have been computed, compute the main problem.
    - Ex: a strictly increasing subsequence does not have to be contiguous and therefore, as long as a value is greater than the last value in the increasing strictly increasing subsequence, we can either include or not include that value.
      - If it is possible to stop the algorithm in any of the subproblems, then it is also possible to stop in the main problem since we can transition from the main problem to any of the two options.
      - Therefore, the Recurrence Relation is `solution(idx, lastVal) = 1 + solution(idx+1, nums[i]) || solution(idx+1, lastVal)` as long as the value is greater than `lastVal`.
4. **Identify the Base Cases.**
    - Ex: if `idx` is out of bounds or the value is less than `lastVal`.
5. **Decide Implementation: Iteration or Recursion.**
    - The above steps are agnostic concerning whether to be implemented recursively or iteratively.
    - Consider the trade-offs between Iteration and Recursion.

<p align="center">
  <img src="https://cdn-media-1.freecodecamp.org/images/E-2qbrD5g7UtOJIN7ULrdwAdgiL0jAU7uGFH" alt="Trade-off between Recursion and Iteration" width="80%" />
</p>

6. **Add Memoization.**
    - For recursive solutions:
      - Treat memoization as the cache of a function result.
      - Store the function result before every `return` statement.
      - Look up the memory for the function result before any computation.
7. **Determine Time Complexity.**
    - Count the number of states (depends on the number of parameters that change).
      - Ex: O(n)
    - Consider the amount of work done per each state.
      - Ex: O(n)
    - Final Time Complexity = O(n^2).
### Conditions
- Dynamic programming can be used on problems with *optimal substructure* and *overlapping subproblems*.
#### 1. Overlapping Subproblems
- A problem is said to have overlapping subproblems if it can be broken down into subproblems which are reused several times.
  - i.e., subproblems that are repeated.
##### Example of Overlapping Subproblems
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/overlapping-subproblems-example.png" alt="Overlapping Subproblems Example" width="60%" />
</p>

  - In the Fibonacci sequence, to find the fifth Fibonnaci number, `fib(3)` has to be calculated twice, `fib(2)` three times, `fib(1)` twice.
##### Dynamic Programming vs Recursion
- Recursive solutions involve subproblems. However, that does not mean the subproblems overlap.
  - For example, in normal circumstances, merge sort does not have overlapping subproblems.
    - Special Case Ex: `mergeSort([10,24,10,24])`. In this case, dynamic programming can be used.
- Dynamic programming solutions require subproblems to overlap. Need to look for repetition.
#### 2. Optimal Substructure
- A problem is said to have optimal substructure if an optimal solution for the problem can be constructed from optional solutions for its subproblems.
- Ex: the optimal solution of `fib(5)` depends on the optimal solutions of `fib(4)` and `fib(3)`, etc.
##### Example of Optimal Substructure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/optimal-substructure-example.png" alt="Optimal Substructure Example" width="60%" />
</p>

  - The optimal solution for the shortest path from `A` to `D` is `A -> B -> C -> D`.
    - Then optimal solution from `A` to `C` has to be `A -> B -> C`.
    - Then optimal solution from `A` to `B` has to be `A -> B`.
##### Example of Non-Optimal Substructure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/non-optimal-substructure-example.png" alt="Non-Optimal Substructure Example" width="60%" />
</p>

  - The longest simple path from `A` to `C` is `A -> B -> C`.
  - The longest simple path from `C` to `D` is `C -> B -> D`.
  - However, the longest simple path from `A` to `D` is not `A -> B -> C -> B -> D`. The longest simple path is `A -> B -> D`.

## Approach for Solving DP Problems
1. Find a **Recurrence Relation**.
    > a recurrence relation is an equation according to which the nth term of a sequence of numbers is equal to some combination of the previous terms. | Wikipedia
2. Find a **Recursive** solution (top-down).
3. Find a **Recursive + Memoization** solution (top-down).
4. Find an **Iterative + Tabulation** solution (bottom-up).
5. Find an **Iterative + N Variables** solution (bottom-up).

## Dynamic Programming Big O
- The time and space complexity for dynamic programming solutions differ per implementation/approach. But the following is a simple formula for Top-Down dynamic programming.
- `Time Complexity = number of function calls * work done per function call`

## Example - Fibonacci Sequence
### Recursion Solution
- Top-Bottom Approach.
  - Ex: for `fib(7)`, start from `fib(7)` and down to `fib(1)` and `fib(2)`.
- The time complexity of this recursive solution is exponential - O(2<sup>n</sup>) (O(branches<sup>depth</sup>)).
- The problem is that we are repeatedly solving the same subproblems several times (overlapping subproblems).
  - What if we could "remember" all these already calculated values?

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/fibonacci-recursion.png" alt="Fibonacci Recursion" width="60%" />
</p>

```js
const fib = (n) => {
  if (n <= 2) return 1;
  return fib(n-1) + fib(n-2);
}
```
### Top-Down Dynamic Programming (Memoization) Solution
- **Storing the results of expensive function calls and returning the cached result when the same inputs occur again.**
  - Ex: once `fib(3)` is calculated once, there is no need to calculate it again, hence, reducing the time complexity.
    - Solve in order: `fib(6)` &rarr; `fib(5)` &rarr; `fib(4)` &rarr; `fib(3)` &rarr; `fib(2)` &rarr; `fib(1)`.
- ***Solve the subproblems that we need only.***
- ***The time complexity of this memoized solution is linear - O(n).***
  - The fibonacci sequence problem is a 1-dimensional dynamic programming problem.
- *Can have bad space complexity, since a recursion tree that is too deep can lead to stack overflow.*

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/fibonacci-memoization.png" alt="Fibonacci Memoization" width="60%" />
</p>

```js
const fib = (n, memo=[]) => {
  if (memo[n] !== undefined) return memo[n];
  if (n <= 2) return 1;
  
  const result = fib(n-1, memo) + fib(n-2, memo); // Pass through memo to accumulate.
  memo[n] = result; // Store fib(n) at the nth index in the memo.
  
  return result;
}
```
### Bottom-Up Dynamic Programming (Tabulation) Solution
- **Storing the results of a previous result in a "table" (usually an array) starting from the bottom.**
  - Ex: for `fib(6)` start with `fib(1)` and `fib(2)` and add them together and then do `fib(3)`, until we reach `fib(6)`.
    - Solve in order: `fib(1)` &rarr; `fib(2)` &rarr; `fib(3)` &rarr; `fib(4)` &rarr; `fib(5)` &rarr; `fib(6)`.
- ***Solve all subproblems that are encountered.***
- Usually implemented using iteration.
- It is called "tabulation" because the results of the subproblems are store in a table of sorts (Ex: an array).
  - Tabulation is about adding to and iterating through the table.
- ***The time complexity of this tabulation solution is linear - O(n).***
- Better space complexity can be achieved using tabulation.
  - *Unlike the solutions using pure recursion or memoization, tabulation will not reach the maximum call stack because it does not have recursive calls, hence takes less space.*

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/fibonacci-tabulation.png" alt="Fibonacci Tabulation" width="60%" />
</p>

```js
const fib = (n) => {
  if (n <= 2) return 1;
  
  const fibNums = [undefined,1,1]; // "table" to store the data.
  
  for (let i = 3; i <= n; i++) {
    fibNums[i] = fibNums[i-1] + fibNums[i-2];
  }
  
  return fibNums[n];
}
```
#### 2 Ways to Calculate Subproblems for Bottom-Up Approach
##### Bottom-Up Forward Dynamic Programming
- Using the results of several subproblems to calculate the result of the next subproblem.
```js
// subproblem1
//            \
//             >-----> subproblem3
//            /
// subproblem2
```
###### Example
```js
const fib = (n) => {
  if (n <= 2) return 1;
  const fibNums = [undefined,1,1];
  for (let i = 3; i <= n; i++) {
    fibNums[i] = fibNums[i-1] + fibNums[i-2];
  }
  return fibNums[n];
}
```
##### Bottom-Up Backward Dynamic Programming
- Using the reuslt of one subproblem to calculate other subproblems.
```js
//            >-----> subproblem2
//           /
// subproblem1
//           \
//            >-----> subproblem3
```
###### Example
```js
const fib = (n) => {
  if (n <= 2) return 1;
  const fibNums = [undefined,1,1];
  for (let i = 2; i <= n; i++) { // Since we know the result for the subproblem fibNums[i], we can use it to calculate fibNums[i+1] and fibNums[i+2].
    fibNums[i+1] += fibNums[i];
    fibNums[i+2] += fibNums[i];
  }
  return fibNums[n];
}
```

## Other Dynamic Programming Patterns
### 0/1 Knapsack
- Given some quantity of things available to us, use them once or zero times to sum to a particular target.
- Knapsack problems are 2-dimensional dynamic programming problems.
#### Example
- Inputs
  - `coins = [1,2,3]`.
  - `target = 5`.
- Some Possible Outputs
  1. Return true if `target` can be summed up to using any of the coins available (each coin can be used 0 or 1 time). Otherwise, return false.
  2. Return the minimum number of coins required to reach the `target`.
##### Solutions
###### Recursion - O(2<sup>n</sup>)
- Each coin can be used once or not (two branches in the decision tree).
- ***Therefore, the time complexity is O(2<sup>n</sup>) where n is the number of items.***
###### Dynamic Programming - O(n\*t)
- There are two dimensions:
  - The size of the `target`.
  - Whether each coin was used or not.
- ***Therefore, the time complexity is O(n\*t) where n is the number of items and t is the target size.***
- Start at subproblem of `target = 5`. Ex: `solution([1,2,3], 5)`
  - If coin `1` is used, the subproblem becomes `target = 4`.
    - Ex: `solution([2,3], 4)`.
  - If coin `2` is used, the subproblem becomes `target = 3`.
    - Ex: `solution([1,3], 3)`.
  - If coin `3` is used, the subproblem becomes `target = 2`.
    - Ex: `solution([1,2], 2)`.
- Base Cases
  - If `target < 0` or invalid, return false.
  - If `target === 0`, return true.
- Flow Example
  ```js
  // solution([1,2,3], 5) // true
    // solution([2,3], 4) // false (3)
      // solution([3], 2) // false (1)
      // solution([2], 1) // false (2)
    // solution([1,3], 3) // true (6) // Code will stop here and solution will return true.
      // solution([3], 2) // false (4)
      // solution([1], 0) // true (5)
    // solution([1,2], 2) // true (9)
      // solution([2], 1) // false (7)
      // solution([1], 0) // true (8)
  ```
### Unbounded Knapsack
- "Unbounded" refers to the fact that we are allowed to use each item an infinite number of times (not 0 or 1).
#### Example
- Inputs
  - `coins = [1,2,3]`.
  - `target = 5`.
- Output: return true if `target` can be summed up to using any of the coins available (each coin can be used an infinite number of times). Otherwise, return false.
##### Solutions
###### Recursion - O(n<sup>t</sup>)
- Each coin can be used or not at every step (two branches in the decision tree).
  - i.e., each coin can be used however many times needed.
- ***Therefore, the time complexity is O(n<sup>t</sup>) where n is the number of items in `coins` and t is the total number of combinations from `coins` that can be summed up to `amount`.***
###### Dynamic Programming - O(n\*t)
- There are two dimensions:
  - The size of the `target`.
  - Whether each coin was used or not.
- ***Therefore, the time complexity is O(n\*t) where n is the number of items and t is the target size.***

## Reference
[Dynamic Programming - GeeksforGeeks](https://www.geeksforgeeks.org/dynamic-programming/)  
[Top 5 Dynamic Programming Patterns for Coding Interviews - For Beginners](https://www.youtube.com/watch?v=mBNrRy2_hVs&ab_channel=AbdulBari)  
[Recurrence relation - Wikipedia](https://en.wikipedia.org/wiki/Recurrence_relation)  
[Leetcode DP Problem Solution Example](https://leetcode.com/problems/house-robber/discuss/156523/From-good-to-great.-How-to-approach-most-of-DP-problems.)  
[DP is Easy! (part 2). Deriving Time Complexity of Top-Down DP | by Tim Park | Medium](https://medium.com/@timpark0807/dp-is-easy-part-2-74422931dd98)  
[Follow these steps to solve any Dynamic Programming interview problem](https://www.freecodecamp.org/news/follow-these-steps-to-solve-any-dynamic-programming-interview-problem-cc98e508cd0e/)  

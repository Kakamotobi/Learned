# Dynamic Programming

## Table of Contents
- [What is Dynamic Progamming?](#what-is-dynamic-programming)
- [Conditions for Dynamic Programming](#conditions-for-dynamic-programming)
  - [1. Overlapping Subproblems](#1-overlapping-subproblems)
  - [2. Optimal Substructure](#2-optimal-substructure)
- [Example - Fibonacci Sequence](#example---fibonacci-sequence)
  - [Recursion Solution](#recursion-solution)
  - [Memoization Solution](#memoization-solution)
  - [Tabulation Solution](#tabulation-solution)
- [Other Dynamic Programming Patterns](#other-dynamic-programming-patterns)
  - [0/1 Knapsack](#01-knapsack)
  - [Unbounded Knapsack](#unbounded-knapsack)

## What is Dynamic Programming?
> Dynamic Programming is mainly an optimization over plain recursion. Wherever we see a recursive solution that has repeated calls for same inputs, we can optimize it using Dynamic Programming. | GeeksforGeeks
- **A method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions so that we can reuse them.**
  - i.e. Using past knowledge to make solving a future problem easier.
- It is an approach that is applicable to a small subset of problems (most problems cannot be solved with it).
  - Kind of like "divide and conquer" in that it can make a huge difference for those problems that can be solved with it.

## Conditions for Dynamic Programming
- Dynamic programming can be used on problems with *optimal substructure* and *overlapping subproblems*.
### 1. Overlapping Subproblems
- A problem is said to have overlapping subproblems if it can be broken down into subproblems which are reused several times.
  - i.e., subproblems that are repeated.
#### Example of Overlapping Subproblems
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/overlapping-subproblems-example.png" alt="Overlapping Subproblems Example" width="60%" />
</p>

  - In the Fibonacci sequence, to find the fifth Fibonnaci number, `fib(3)` has to be calculated twice, `fib(2)` three times, `fib(1)` twice.
#### Dynamic Programming vs Recursion
- Recursive solutions involve subproblems. However, that does not mean the subproblems overlap.
  - For example, in normal circumstances, merge sort does not have overlapping subproblems.
    - Special Case Ex: `mergeSort([10,24,10,24])`. In this case, dynamic programming can be used.
- Dynamic programming solutions require subproblems to overlap. Need to look for repetition.
### 2. Optimal Substructure
- A problem is said to have optimal substructure if an optimal solution for the problem can be constructed from optional solutions for its subproblems.
- Ex: the optimal solution of `fib(5)` depends on the optimal solutions of `fib(4)` and `fib(3)`, etc.
#### Example of Optimal Substructure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/optimal-substructure-example.png" alt="Optimal Substructure Example" width="60%" />
</p>

  - The optimal solution for the shortest path from `A` to `D` is `A -> B -> C -> D`.
    - Then optimal solution from `A` to `C` has to be `A -> B -> C`.
    - Then optimal solution from `A` to `B` has to be `A -> B`.
#### Example of Non-Optimal Substructure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/non-optimal-substructure-example.png" alt="Non-Optimal Substructure Example" width="60%" />
</p>

  - The longest simple path from `A` to `C` is `A -> B -> C`.
  - The longest simple path from `C` to `D` is `C -> B -> D`.
  - However, the longest simple path from `A` to `D` is not `A -> B -> C -> B -> D`. The longest simple path is `A -> B -> D`.

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
### Memoization Solution
- Top-Bottom Approach.
- **Storing the results of expensive function calls and returning the cached result when the same inputs occur again.**
  - Ex: once `fib(3)` is calculated once, there is no need to calculate it again, hence, reducing the time complexity.
- ***The time complexity of this memoized solution is linear - O(n).***
  - The fibonacci sequence problem is a 1-dimensional dynamic programming problem.

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
### Tabulation Solution
- Bottom-Up Approach.
  - Ex: for `fib(6)` start with `fib(1)` and `fib(2)` and add them together and then do `fib(3)`, until we reach `fib(6)`.
- **Storing the results of a previous result in a "table" (usually an array) starting from the bottom.**
- Usually implemented using iteration.
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
- Each coin can be used or not (two branches in the decision tree).
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
###### Recursion - O(2<sup>n</sup>)
###### Dynamic Programming - O(n\*t)

## Reference
[Dynamic Programming - GeeksforGeeks](https://www.geeksforgeeks.org/dynamic-programming/)  
[Top 5 Dynamic Programming Patterns for Coding Interviews - For Beginners](https://www.youtube.com/watch?v=mBNrRy2_hVs&ab_channel=AbdulBari)  

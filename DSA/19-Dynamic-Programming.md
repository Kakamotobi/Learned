# Dynamic Programming

## Table of Contents
- [What is Dynamic Progamming?](#what-is-dynamic-programming)
- [Conditions for Dynamic Programming](#conditions-for-dynamic-programming)
  - [1. Overlapping Subproblems](#1-overlapping-subproblems)
  - [2. Optimal Substructure](#2-optimal-substructure)
- [Example](#example)

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

## Example
### Fibonacci Sequence
#### Recursion Solution
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/fibonacci-recursion.png" alt="Fibonacci Recursion" width="60%" />
</p>

```js
const fib = (n) => {
  if (n <= 2) return 1;
  return fib(n-1) + fib(n-2);
}
```
- The time complexity of this recursive solution is exponential - O(2<sup>n</sup>) (O(branches<sup>depth</sup>)).
- The problem is that we are repeatedly solving the same subproblems several times (overlapping subproblems).
  - What if we could "remember" all these already calculated values?
#### Memoization Solution
- **Memoization**
  - Storing the results of expensive function calls and returning the cached result when the same inputs occur again.
  - Ex: once `fib(3)` is calculated once, there is no need to calculate it again, hence, reducing the time complexity.
  - The time complexity of this memoized solution is linear - O(n).

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

## Reference
[Dynamic Programming - GeeksforGeeks](https://www.geeksforgeeks.org/dynamic-programming/)  

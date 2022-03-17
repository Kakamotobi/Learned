# Dynamic Programming

## Table of Contents
- [What is Dynamic Progamming?](#what-is-dynamic-programming)
- [Conditions for Dynamic Programming](#conditions-for-dynamic-programming)
  - [1. Overlapping Subproblems](#1-overlapping-subproblems)
  - [2. Optimal Substructure](#optimal-substructure)

## What is Dynamic Programming?
- **A method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions.**
- An approach that is applicable to a small subset of problems (most problems cannot be solved with it).
  - Kind of like "divide and conquer" in that dynamic programming can make a huge difference for those problems that can be solved with it.

## Conditions for Dynamic Programming
- Dynamic programming can be used on problems with *optimal substructure* and *overlapping subproblems*.
### 1. Overlapping Subproblems
- A problem is said to have overlapping subproblems if it can be broken down into subproblems which are reused several times.
  - i.e., subproblems that are repeated.
#### Example
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/fibonacci-sequence-example.png" alt="Fibonacci Sequence Example" width="60%" />
</p>

  - In the Fibonacci sequence, to find the fifth Fibonnaci number, `fib(3)` has to be calculated twice, `fib(2)` three times, `fib(1)` twice.
#### Dynamic Programming vs Recursion
- Recursive solutions involve subproblems. However, that does not mean the subproblems overlap.
  - For example, in normal circumstances, merge sort does not have overlapping subproblems.
    - Special Case Ex: `mergeSort([10,24,10,24])`. In this case, dynamic programming can be used.
- Dynamic programming solutions require subproblems to overlap. Need to look for repetition.
### 2. Optimal Substructure




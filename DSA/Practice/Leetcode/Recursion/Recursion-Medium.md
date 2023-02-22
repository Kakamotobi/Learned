# Recursion - Medium

## Table of Contents
- [894. All Possible Full Binary Trees](#894-all-possible-full-binary-trees)

## [894. All Possible Full Binary Trees](https://leetcode.com/problems/all-possible-full-binary-trees)
### Solution
```js
// TC: O(2^n), SC: O(n)

const allPossibleFBT = (n) => {
  const memo = {};

  // Return an array of FBTs using `n` number of nodes.
  const helper = (numNodes) => {
    if (numNodes === 0) return [];
    if (numNodes === 1) return [new TreeNode(0)];
    if (memo[numNodes] !== undefined) return memo[numNodes];

    const res = [];

    // 1 node is used for root. So, we're left with n-1 nodes (therefore, `left < n` and `n - 1`).
    for (let leftNumNodes = 0; leftNumNodes < numNodes; leftNumNodes++) { // number of nodes in the left tree.
      let rightNumNodes = numNodes - 1 - leftNumNodes; // number of nodes in the right tree.

      // All possible FBTs that can be made are combinations of the root and one from `leftTrees` and one from `rightTrees`.
      const leftTrees = helper(leftNumNodes);
      const rightTrees = helper(rightNumNodes);

      for (let lT of leftTrees) {
        for (let rT of rightTrees) {
          res.push(new TreeNode(0, lT, rT)); // Created a FBT.
        }
      }
    }

    memo[numNodes] = res;

    return res;
  }

  return helper(n);
};
```

# Tree

## 1. [Same Tree](https://leetcode.com/problems/same-tree/)
- DFS both trees at the same time and compare structure and value.
```js
// TC: O(n), SC: O(1)

const isSameTree = (p, q) => {
  if (p === null && q === null) return true;
  if (p === null || q === null) return false;
  if (p.val !== q.val) return false;
  
  return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

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

## 2. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
### Solution 1 - DFS
```js
const maxDepth = (root) => {
  if (root === null) return 0;
  
  const leftDepth = maxDepth(root.left);
  const rightDepth = maxDepth(root.right);
  return 1 + Math.max(leftDepth, rightDepth);
}
```
### Solution 2 - BFS
```js
const maxDepth = (root) => {
  let depth = 0;
  
  // level represents all the nodes in the current level.
  let level = root ? [root] : [];
  while (level.length > 0) {
    depth++;
    
    // Gather the nodes for the next level.
    const nextLevel = [];
    for (let node of level) {
      if (node.left) nextLevel.push(node.left);
      if (node.right) nextLevel.push(node.right);
    }
    
    level = nextLevel;
  }
  
  return depth;
}
```

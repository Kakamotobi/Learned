# Binary Tree - Easy

## 104. Maximum Depth of Binary Tree
- Input: `root` of a binary tree.
- Output: the maximum depth of the binary tree.
### Solution
```js
const maxDepth = (root) => {
  if (root === null) return 0;
  else {
    let leftDepth = maxDepth(root.left);
    let rightDepth = maxDepth(root.right);
    return 1 + Math.max(leftDepth, rightDepth);
  }
}
```

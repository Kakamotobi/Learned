# Binary Tree - Medium

## 102. Binary Tree Level Order Traversal
- Input: `root` of a binary tree.
- Output: return the level order traversal of its nodes' values.
### Example
```js
// Input: [3,9,20,null,null,15,7]
// Output: [[3], [9,20], [15,7]]
```
### Solution
```js
// Use BFS to traverse the tree.
// Before dequeueing, the queue's length indicates the number of nodes in that level (also applies to the root level).

const levelOrder = (root) => {
  const values = [];
  // Initialize queue with root.
  const queue = [root];
  // While queue is not empty.
  while (queue.length) {
    const level = [];
    // Preserve the number of nodes in this level currently (ensures that we iterate through one level at a time).
    const qLength = queue.length;
    // Iterate through the nodes in this level only.
    for (let i = 0; i < qLength; i++) {
      const currNode = queue.shift();
      // If currNode is not null.
      if (currNode !== null) {
        // Push currNode's val to level.
        level.push(currNode.val);
        // Push the next level nodes to the queue.
        queue.push(currNode.left, currNode.right);
      }
    }
    // If level is not empty, push level to values.
    if (level.length) values.push(level);
  }
  return values;
}
```

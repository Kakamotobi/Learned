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
- Use BFS to traverse the tree.
- Before dequeueing, the queue's length represents the number of nodes in that level (also applies to the root level).
```js
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

## 105. Construct Binary Tree from Preorder and Inorder Traversal
- Input: two integer arrays `preorder` and `inorder` representing the preorder traversal and inorder traversal, respectively, of a binary tree.
- Output: construct and return the binary tree.
- Constraints
  - `1 <= preorder.length <= 3000`.
  - `inorder.length === preorder.length`.
  - `-3000 <= preorder[i], inorder[i] <= 3000`.
  - `preorder` and `inorder` consist of unique values.
  - Each value of `inorder` also appears in `preorder`.
  - `preorder` is guaranteed to be the preorder traversal of the tree.
  - `inorder` is guaranteed to be the inorder traversal of the tree.
### Solution
- `preorder[0]` is always the root of the tree.
- The nodes before `preorder[0]` in `inorder` are on the left subtree of the root. The nodes after `preorder[0]` in `inorder` are on the right subtree of the root.
  - Count the number of nodes on each side (in `inorder`) and use that to "reduce" `preorder` and `inorder` for recursive calls for left and right.
```js
const buildTree = (preorder, inorder) => {
  // If preorder and inorder are empty, return null.
  if (!preorder.length && !inorder.length) return null;
  
  // Create node.
  const root = new TreeNode(preorder[0]);
  // Get the node's position in inorder.
  const pos = inorder[preorder[0]];

  // For preorder of root.left, start from the node after root to pos (inclusive).
  // For inorder of root.left, start from the first node to pos (exclusive).
  root.left = buildTree(preorder.slice(1, pos+1), inorder.slice(0, pos));
  // For preorder of root.right, start from the node after pos to the end.
  // For inorder of root.right, start from the node after pos to the end..
  root.right = buildTree(preorder.slice(pos+1), inorder.slice(pos+1));
  
  return root;
}
```






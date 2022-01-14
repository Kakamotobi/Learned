# Binary Search Trees - Easy

## 938. Range Sum of BST
- Input: the `root` node of a BST, two integers `low` and `high`.
- Output: return the sum of values of all nodes with a value in the inclusive range `[low, high]`.
- Constraints
  - The number of nodes in the tree is in the range `[1, 2 * 10^4]`.
  - `1 <= Node.val <= 10^5`.
  - `1 <= low <= high <= 10^5`.
  - All `Node.val` are unique.
### Solution 1 - BFS
```js
const rangeSumBST = (root, low, high) => {
  let sum = 0;
  const queue = [root];
  let currNode;
  while (queue.length !== 0) {
    currNode = queue.shift();
    if (currNode.val >= low && currNode.val <= high) sum += currNode.val;
    if (currNode.left) queue.push(currNode.left);
    if (currNode.right) queue.push(currNode.right);
  }
  return sum;
}
```
### Soution 2 - DFS Preorder
```js
const rangeSumBST = (root, low, high) => {
  let sum = 0;
  let currNode = root;
  function traverse(node) {
     if (node.val >= low && node.val <= high) sum += node.val;
     if (node.left) traverse(node.left);
     if (node.right) traverse(node.right);
  }
  traverse(currNode);
  return sum;
}
```
### Solution 3 - DFS Postorder
```js
const rangeSumBST = (root, low, high) => {
  let sum = 0;
  let currNode = root;
  function traverse(node) {
    if (node.left) traverse(node.left);
    if (node.right) traverse(node.right);
    if (node.val >= low && node.val <= high) sum += node.val;
  }
  traverse(currNode);
  return sum;
}
```
### Solution 4 - DFS Inorder
```js
const rangeSumBST = (root, low, high) => {
  let sum = 0;
  let currNode = root;
  function traverse(node) {
    if (node.left) traverse(node.left);
    if (node.val >= low && node.val <= high) sum += node.val;
    if (node.right) traverse(node.right);
  }
  traverse(currNode);
  return sum;
}
```

## 897. Increasing Order Search Tree
- Input: the `root` node of a BST.
- Output: rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.
- Constraints
  - Number of nodes will range between `[1, 100]`.
  - `0 <= Node.val <= 1000`.
### Solution - DFS Inorder
```js
const increasingBST = (root) => {
  // Initialize new root.
  let newRoot;
  // Initialize current node for the new tree.
  let currNodeForNewTree;
  // Inorder Traversal Helper Function
  function inorderTraversal(node) {
    // If the node has a left property, recursively call its left property.
    if (node.left) inorderTraversal(node.left);
    
    // If newRoot has not been set yet, initialize newRoot and currNodeForNewTree to be the node.
    if (!newRoot) {
      newRoot = node;
      currNodeForNewTree = newRoot;
    }
    // Else (newRoot has been set already).
    else {
      // Set currNode's right property to the node.
      currNode.right = node;
      // Update currNode.
      currNode = currNode.right;
      // Set currNode's left to be null.
      currNode.left = null;
    }
    
    // If the node has a right property, recursively call its right property.
    if (node.right) inorderTraversal(node.right);
  }
  inorderTraversal(root);
  return newRoot;
}
```

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

## 617. Merge Two Binary Trees
- Input: two binary trees `root1` and `root2`.
- Output: merge the trees so that the nodes are summed up. If one of the tree's node is null, use the other node. Then return the merged tree.
### Solution
```js
const mergeTrees = (root1, root2) => {
  // Base Cases
  if (root1 === null) return root2;
  if (root2 === null) return root1;
  
  else {
    root1.val = root1.val + root2.val;
    root1.left = mergeTrees(root1.left, root2.left);
    root1.right = mergeTrees(root1.right, root2.right);
    return root1;
  }
  
}
```

## 226. Invert Binary Tree
- Input: `root` of a binary tree.
- Output: invert the tree and return its root.
### Solution 1 - Recursion
```js
const invertTree = (root) => {
  if (root === null) return null;
  [root.left, root.right] = [root.right, root.left];
  invertTree(root.left);
  invertTree(root.right);
  return root;
}
```
### Solution 2 - DFS preorder using stack
```js
const invertTree = (root) => {
  const stack = [root];
  while (stack.length) {
    const currNode = stack.pop();
    if (currNode !== null) {
      [currNode.left, currNode.right] = [currNode.right, currNode.left];
      stack.push(currNode.right, currNode.left);
    }
  }
  return root;
}
```
### Solution 3 - BFS
```js
const invertTree = (root) => {
  const queue = [root];
  while (queue.length) {
    const currNode = queue.shift();
    if (currNode !== null) {
      [currNode.left, currNode.right] = [currNode.right, currNode.left];
      queue.push(currNode.left, currNode.right);
    }
  }
  return root;
}
```

## 100. Same Tree
- Input: the roots of two binary trees `q` and `p`.
- Output: return true if they are identical in structure and value.
### Solution
```js
const isSameTree = (p, q) => {
  // If p and q are both null, return true.
  if (p === null && q === null) return true;
  // If either p or q is null (but not both), return false.
  if (p === null || q === null) return false;
  // If p and q's values are not equal, return false.
  if (p.val !== q.val) return false;
  
  return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

## 572. Subtree of Another Tree
- Input: the root of two binary trees `root` and `subRoot`.
- Output: return true if there is a subtree of `root` with the same structure and node values of `subRoot`.
- Constraints
  - Number of nodes in the root tree is in the range `[1, 2000]`.
  - Number of nodes in the subRoot tree is in the range `[1, 1000]`.
  - `-10^4 <= root.val <= 10^4`
  - `-10^4 <= subRoot.val <= 10^4`
### Solution
```js
// Traverse through the tree.
// If encountered a node with a value the same as subRoot's val, check for the whole subtree.

const isSubtree = (root, subRoot) => {
  // Check if subtree is identical.
  function isSame(treeNode, subRootNode) {
    if (treeNode === null && subRootNode === null) return true;
    if (treeNode === null || subRootNode === null) return false;
    if (treeNode.val !== subRootNode.val) return false;
    return isSame(treeNode.left, subRootNode.left) && isSame(treeNode.right, subRootNode.right);
  }
  
  // Traverse the tree and check for subtree.
  function traverse(treeNode) {
    if (treeNode === null) return false;
    
    // If treeNode's val is the same as subRoot's val.
    if (treeNode.val === subRoot.val) {
      // If subtree is identical, return true.
      if (isSame(treeNode, subRoot)) return true;
    }
    
    return traverse(treeNode.left) || traverse(treeNode.right);
  }
  
  return traverse(root);
}
```

## 1022. Sum of Root to Leaf Binary Numbers
- Input: `root` of a binary tree where each node has a value of `0` or `1`.
  - Each root-to-leaf path represents a binary number starting with the most significant bit.
- Output: for all leaves in the tree, consider the numbers represented by the path from the root to that leaf. Return the sum of these numbers.
### Example
```js
// Input: [1,0,1,0,1,0,1]
// Output: 22
```
### Solution
- Need to use preorder traversal on the tree.
- Need a way to keep track of the root to leaf path (use a string).
- Need a sum variable.
```js
const sumRootToLeaf = (root) => {
  function preorder(root, rootToLeafPath) {
    if (root === null) return;
    if (root.left === null && root.right === null) {
      sum += parseInt(rootToLeafPath + root.val, 2);
      return;
    }
    preorder(root.left, rootToLeafPath + root.val);
    preorder(root.right, rootToLeafPath + root.val);
  }
  
  let sum = 0;
  preorder(root, "");
  return sum;
}
```

## 112. Path Sum
- Input: `root` of a binary tree, and an integer `targetSum`.
- Output: return `true` if the tree has a root-to-leaf path such that sum of the values along the path equals `targetSum`.
### Solution
- Need to traverse the tree using DFS.
- Need a way to keep track of the sum of the root to leaf path.
  - Do so by subtracting node's val from `targetSum` and checking to see if `targetSum === 0` at the leaf.
```js
const hasPathSum = (root, targetSum) => {
  if (root === null) return false;
  targetSum -= root.val;
  if (root.left === null && root.right === null) {
    return targetSum === 0;
  }
  return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
}
```

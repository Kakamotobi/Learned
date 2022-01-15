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

## 108. Convert Sorted Array to Binary Search Tree
- Input: an integer array `nums` sorted in **ascending order**.
- Output: return the array converted into a ***height-balanced*** binary search tree.
- Constraints
  - `1 <= nums.length <= 10^4`.
  - `-10^4 <= nums[i] <= 10^4`.
  - `nums` is sorted in a strictly increasing order.
### Example
```js
// Input: [-10,-3,0,5,9]
// Output: [0,-3,9,-10,null,5] or [0,-10,5,null,-3,null,9]

// sortedArrayToBST(nums, 0, 4) // root: 0 // (11) return 0.
   // sortedArrayToBST(nums, 0, 1) // root's left // currNode = -10 // (5) return -10.
      // sortedArrayToBST(nums, 0, -1) // -10's left // base case reached. // (1) return null.
      // sortedArrayToBST(nums, 1, 1) // -10's right // currNode = -3 // (4) return -3.
         // sortedArrayToBST(nums, 1, 0) // -3's left // base case reached. // (2) return null.
         // sortedArrayToBST(nums, 2, 1) // -3's right // base case reached. // (3) return null.
   // sortedArrayToBST(nums, 3, 4) // root's right // currNode = 5 // (10) return 5.
      // sortedArrayToBST(nums, 3, 2) // 5's left // base case reached. // (6) return null.
      // sortedArrayToBST(nums, 4, 4) // 5's right // currNode = 9 // (9) return 9.
         // sortedArrayToBST(nums, 4, -3) // 9's left // base case reached. // (7) return null.
         // sortedArrayToBST(nums, 5, 4) // 9's left // base case reached. // (8) return null.
```
### Solution
```js
// Set default pointer values.
const sortedArrayToBST = (nums, start=0, end=nums.length-1) => {
  // Base Case
  if (start > end) return null;

  // Calculate the mid index between start and end.
  const mid = Math.floor((start+end)/2);
  // Create node (this will be the root in the first recursion).
  const currNode = new TreeNode(nums[mid]);

  // Recursive call on left side.
  currNode.left = sortedArrayToBST(nums, start, mid-1);
  // Recursive call on right side.
  currNode.right = sortedArrayToBST(nums, mid+1, end);
  
  return currNode;
}

// Binary Tree Node
// function TreeNode(val, left, right) {
//   this.val = (val===undefined ? 0 : val);
//   this.left = (left===undefined ? null : left);
//   this.right = (right===undefined ? null : right);
// }
```

## 530. Minimum Absolute Difference in BST
- Input: the `root` of a binary tree.
- Output: return the minimum absolute difference between the values of any two different nodes in the tree.
- Constraints
  - Number of nodes is between `[2, 10^4]`.
  - `0 <= Node.val <= 10^5`.
### Example
```js
// Input: [4,2,6,1,3]
// Output: 1

// getMinimumDifference(4) // minDiff = 1 // prevNodeValue = 4 // (4)
   // getMinimumDifference(2) // minDiff = 1 // prevNodeValue = 2 // (2)
      // getMinimumDifference(1) // minDiff = Infinity // prevNodeValue = 1 // (1)
      // getMinimumDifference(3) // minDiff = 1 // prevNodeValue = 3 // (3)
   // getMinimumDifference(6) // minDiff = 1 // prevNodeValue = 6 // (5)
```
### Solution
```js
// Inorder DFS on a BST will allow us to traverse through each node in sorted order.
// This means that |node3.val - node1.val| will not be less than |node2.val - node1.val|.
// Therefore, we only need to get the difference between adjacent node values.

const getMinimumDifference = (root) => {
  let minDiff = Infinity;
  let prevNodeVal = -Infinity;
  function inorderTraverse(node) {
    if (node.left) inorderTraverse(node.left);
    // Update minDiff.
    minDiff = Math.min(minDiff, node.val - prevNodeVal);
    // Store current node's val for next comparison.
    prevNodeVal = node.val;
    if (node.right) inorderTraverse(node.right);
  }
  inorderTraverse(root);
  return minDiff;
}
```

## 235. Lowest Common Ancestor of a Binary Search Tree
- Input: `root` of a BST, two nodes `p` and `q`.
- Output: return the lowest common ancestor shared by `p` and `q` in the BST.
- Constraints
  - Number of nodes range between `[2, 10^5]`.
  - `-10^9 <= Node.val <= 10^9`.
  - All `Node.val` are unique.
  - `p != q`.
  - `p` and `q` are guaranteed to exist in the BST.
### Solution
```js
// Initialize variable to keep track of common ancestor with the root.
// Keep "pushing" down the variable to the node below until p and q split up directions.

const lowestCommonAncestor = (root, p, q) => {
  let currNode = root;
  while (currNode) {
    // If currNode's value is greater than both p and q's value, move on to currNode's left node.
    if (currNode.val > p.val && currNode.val > q.val) currNode = currNode.left;
    // Else if currNode's value is less than both p and q's value, move on to currNode's right node.
    else if (currNode.val < p.val && currNode.val < q.val) currNode = currNode.right;
    // Else, p and q split up directions.
    else break;
  }
  return currNode;
}
```

## 501. Find Mode in Binary Search Tree
- Input: `root` of a BST with duplicates.
- Output: return all the modes (i.e., the most frequently occured element) in the BST.
- Constraint:
  - `Node.left.val <= Node.val <= Node.right.val`.
### Solution
```js
const findMode = (root) => {
  const modes = [];
  const freq = {};
  let maxMode = -Infinity;
  
  function preorder(node) {
    freq[node.val] = ++freq[node.val] || 1;
    if (freq[node.val] > maxMode) maxMode = freq[node.val];
    if (node.left) preorder(node.left);
    if (node.right) preorder(node.right);
  }
  preorder(root);
  
  for (let key in freq) {
    if (freq[key] === maxMode) modes.push(key);
  }
  return modes;
}
```


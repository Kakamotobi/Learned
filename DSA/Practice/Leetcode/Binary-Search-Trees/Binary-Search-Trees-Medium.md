# Binary Search Trees - Medium

## 1382. Balance a Binary Search Tree
- Input: `root` of a BST.
- Output: return a balanced binary search tree with the same node values.
- Constraints
  - Number of nodes range between `[1, 10^4]`.
  - `1 <= Node.val <= 10^5`.
### Solution - inorder DFS and binary search
- Record the node values in sorted order.
- Use binary search on the sorted array to construct a balanced BST.
```js
const balanceBST = (root) => {
  const values = [];
  function inorder(node) {
    if (node.left) inorder(node.left);
    values.push(node.val);
    if (node.right) inorder(node.right);
  }
  inorder(root);
  
  // Ultimately returns the root.
  function constructBST(values, start=0, end=values.length-1) {
    if (start > end) return null;
    const mid = Math.floor((start+end)/2);
    const currNode = new TreeNode(values[mid]);
    currNode.left = constructBST(values, start, mid-1);
    currNode.right = constructBST(values, mid+1, end);
    return currNode;
  }
  
  return constructBST(values);
}
```

## 230. Kth Smallest Element in a BST
- Input: `root` of a BST, an integer `k`.
- Output: return the `k`th smallest value (1-indexed) of all the values of the nodes in the tree.
### Solution
```js
const kthSmallest = (root, k) => {
  const values = [];
  function inorder(node) {
    if (values.length < k) {
      if (node.left) inorder(node.left);
      values.push(node.val);
      if (node.right) inorder(node.right);
    }
  }
  inorder(root);
  return values[k-1];
}
```

## 1008. Construct Binary Search Tree from Preorder Traversal
- Input: an array of integers `preorder` that represents the preorder traversal of a BST.
- Output: construct the BST and return its root.
- Constraints
  - `1 <= preorder.length <= 100`.
  - `1 <= preorder[i] <= 1000`.
  - All the values of `preorder` are unique.
### Solution
```js
const bstFromPreorder = (preorder) => {
  const root = new TreeNode(preorder[0]);
  
  function insert(node, num) {
    if (num < node.val) {
      if (node.left) insert(node.left, num);
      else node.left = new TreeNode(num);
    }
    else if (num > node.val) {
      if (node.right) insert(node.right, num);
      else node.right = new TreeNode(num);
    }
  }
  
  for (let i = 1; i < preorder.length; i++) {
    insert(root, num);
  }
  
  return root;
}
```

## 98. Validate Binary Search Tree
- Input: `root` of a binary tree.
- Output: determine if it is a BST.
### Example
```js
// Input (BFS traversal): [5,1,7,null,null,3,9]

// isValidBST(root) // (10) return false.
   // validate(5, -Infinity, Infinity) // (9) return true && false.
      // validate(1, -Infinity, 5) // (3) return true && true.
         // validate(null, -Infinity, 1) // base case reached (node === null) // (1) return true.
         // validate(null, 1, 5) // base case reached (node === null) // (2) return true.
      // validate(7, 5, Infinity) // (8) return false && true.
         // validate(3, 5, 7) // base case reached (node not in range) // (4) return false.
         // validate(9, 7, Infinity) // (7) return true && true.
            // validate(null, 7, 9) // base case reached (node === null) // (5) return true.
            // validate(null, 9, Infinity) // base case reached (node === null) // (6) return true.
```
### Solution
```js
// For each node, its left property has to be less than node.val, and its right property has to be greater than node.val. Otherwise, it's not a BST.
// Therefore, for each node's recursive call, pass in its valid range (min, max).

const isValidBST = (root) => {
  function validate(node, min, max) {
    if (node === null) return true;
    // If node's val is not in the valid range.
    if (node.val <= min || node.val >= max) return false;
    return validate(node.left, min, node.val) && validate(node.right, node.val, max)
  }
  return validate(root, -Infinity, Infinity);
}
```

## 538. Convert BST to Greater Tree
- Input: `root` of a BST.
- Output: return the `root` after changing all node's values to its value plus the sum of all nodes greater than it.
### Solution
- Inorder traversal starting from the largest number.
- While traversing, update each node's val to the sum of what came before it.
  - Need a variable to keep track of this sum.
```js
const convertBST = (root) => {
  let sum = 0;
  reversedInorder(root);
  return root;
  
  function reversedInorder(node) {
    if (node.right) reversedInorder(node.right);
    node.val = node.val + sum;
    sum = node.val;
    if (node.left) reversedInorder(node.left);
  }
}
```

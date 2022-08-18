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
// TC: O(n), SC: O(n)

const maxDepth = (root) => {
  if (root === null) return 0;
  
  const leftDepth = maxDepth(root.left);
  const rightDepth = maxDepth(root.right);
  return 1 + Math.max(leftDepth, rightDepth);
}
```
### Solution 2 - BFS
```js
// TC: O(n), SC: O(n)

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

## 3. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
### Solution 1 - DFS Postorder
- Use DFS Postorder to traverse the leftmost node, then the rightmost node, then the node.
```js
// TC: O(n), SC: O(n)

const invertTree = (root) => {
  const dfs = (node) => {
    if (node === null) return;
    
    if (node.left) dfs(node.left);
    if (node.right) dfs(node.right);
    
    [node.left, node.right] = [node.right, node.left];
  }
  
  dfs(root);
  
  return root;
}
```
### Solution 2 - BFS
```js
// TC: O(n), SC: O(n)

const invertTree = (root) => {
  if (root === null) return null;
  
  const queue = [root];
  let currNode;
  while (queue.length > 0) {
    currNode = queue.shift();
    [currNode.left, currNode.right] = [currNode.right, currNode.left];
    
    if (currNode.left) queue.push(currNode.left);
    if (currNode.right) queue.push(currNode.right);
  }
  
  return root;
}
```

## 4. [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
- Traverse through `root` and if the current node's value is the same as `subRoot`'s (head's) value.
```js
// TC: O(n*m), SC: O(1)
  // n: the number of nodes in root.
  // m: the number of nodes in subRoot.

const isSubtree = (root, subRoot) => {
  // If the nodes are the same, recursive call will keep going until the end (both nodes are null).
  const isSame = (treeNode, subtreeNode) => {
    if (treeNode === null && subtreeNode === null) return true;
    if (treeNode === null || subtreeNode === null) return false;
    if (treeNode.val !== subtreeNode.val) return false;
    return isSame(treeNode.left, subtreeNode.right) && isSame(treeNode.right, subtreeNode.right);
  }
  
  // Return true if subtree exists, false if not.
  // The `||` guarantees true if a subtree has been found anywhere in the tree.
  const dfs = (node) => {
    if (node === null) return false;
    if (node.val === subRoot.val && isSame(node, subRoot)) return true;
    return dfs(node.left) || dfs(node.right);
  }
  
  return dfs(root);
}
```

## 5. [Lowest Common Ancestor of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
- The `root` is guaranteed to be a common ancestor so start loop from there.
- If the current node is either `p` or `q`, return current node.
- If `p` or `q` split up from current node, return current node.
```js
// TC: O(log n), SC: O(1)

const lowestCommonAncestor = (root, p, q) => {
  let currNode = root;
  while (true) {
    if (p.val < root.val && q.val < root.val) currNode = currNode.left;
    else if (p.val > root.val && q.val > root.val) currNode = currNode.right;
    else return currNode;
  }
}
```

## 6. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- Use BFS.
  - At the start of an iteration, the nodes in `queue` represents all the nodes in the current level.
  - However, the next level's nodes are added to `queue` in the process.
```js
// TC: O(n), SC: O(n)

const levelOrder = (root) => {
  const ans = [];
  
  const queue = [root];
  
  while (queue.length > 0) {
    const currLevel = [];
    const numNodesInCurrLevel = queue.length;
    
    for (let i = 0; i < numNodesInCurrLevel; i++) {
      const currNode = queue.shift();
      
      if (currNode !== null) {
        currLevel.push(currNode.val);
        queue.push(currNode.left, currNode.right);
      }
    }
    
    if (currLevel.length > 0) ans.push(currLevel);
  }
  
  return ans;
}
```

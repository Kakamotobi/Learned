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

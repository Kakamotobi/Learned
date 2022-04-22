# Trees and Graphs

## 4.1 Route Between Nodes
- Given a directed graph, design an algorithm to find out whether there is a route between two nodes.
### Solutions
#### Approach 1 - O(n)
- Use depth-first search to traverse the graph starting from the first node.
- If the second node is encountered while traversing, return true. Otherwise, return false.
```js
const routeExists = (adjList, startingNode, endingNode) => {
  if (startingNode === endingNode) return true;
  
  const visited = {};
  const dfs = (node) => {
    visited[node] = true;
    if (node === endingNode) return true;
    for (let neighbor of adjList[node]) {
      if (!visited[neighbor]) traverse(neighbor);
    }
  }
  
  return dfs(startingNode) === true ? true : false;
}
```
#### Approach 2 - O(n)
- Use breadth-first search to do the same as Approach 1.
```js
const routeExists = (adjList, startingNode, endingNode) => {
  if (startingNode === endingNode) return true;
  
  const visited = {};
  const queue = [startingNode];
  visited[startingNode] = true;
  let currNode;
  while (queue.length) {
    currNode = queue.shift();
    for (let neighbor of adjList[currNode]) {
      if (neighbor === endingNode) return true;
      if (!visited[neighbor]) queue.push(neighbor);
      visited[neighbor] = true;
    }
  }
  return false;
}
```

## 4.2 Minimal Tree
- Given an array of unique values in ascending order, create a binary search tree with minimal height.
### Solution
- Minimal height implies that the root node will be the value in the middle of the array.
  - Hence, the value in the middle will always be the root node of a subarray.
```js
const createMinimalBST = (arr) => {
  const helper = (arr, start, end) => {
    if (start > end) return null;
    const mid = (start + end) / 2;
    const node = new Node(arr[mid]);
    node.left = helper(arr, start, mid - 1);
    node.right = helper(arr, mid + 1, end);
    return node;
  }
  
  return helper(arr, 0, arr.length - 1);
}
```

## 4.4 Check Balanced
- Check if a binary tree is balanced, where balanced means that the difference between the heights of the subtrees of ANY node is not greater than 1.
### Solution
#### Approach 1 - O(n log n)
- Calculate the difference between the heights of the subtrees for each node.
- If any of the node has subtrees with a height difference greater than 1, return false.
```js
const checkBTBalanced = (root) => {
  const calcHeight = (node) => {
    if (node === null) return -1;
    return Math.max(calcHeight(node.left), calcHeight(node.right)) + 1;
  }
  const isBalanced = (node) => {
    if (node === null) return true;
    
    const heightDiff = Math.abs(calcHeight(node.left) - calcHeight(node.right));
    if (heightDiff > 1) return false;
    else return isBalanced(node.left) && isBalanced(node.right);
  }
  
  return isBalanced(root);
}
```

# 4.5 Validate BST
- Given a binary tree, check if it is a binary search tree.
### Solution - O(n)
- Inorder traverse the binary tree and collect values (O(n)).
- If the collected values are sorted, return true (O(n)).
- Assumption: there are no duplicates.
```js
const validateBST = (root) => {
  const visited = [];
  const inorder = (node) => {
    if (node === null) return;
    inorder(node.left);
    visited.push(node.val);
    inorder(node.right);
  }
  
  for (let i = 0; i < visited.length - 1; i++) {
    if (visited[i] > visited[i+1]) return false;
  }
  return true;
}
```

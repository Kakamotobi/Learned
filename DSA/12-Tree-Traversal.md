# Tree Traversal

## Table of Content
- [The Gist: how to visit every node once](#the-gist-how-to-visit-every-node-once)
- [Breadth-First Search (BFS)](#breadth-first-search-bfs)
  - [BFS Implementation](#bfs-implementation)
  - [BFS for Binary Search Tree Flow Example](#bfs-for-binary-search-tree-flow-example)
- [Depth-First Search (DFS)](#depth-first-search-dfs)
  - [Three Orders of DFS](#three-orders-of-dfs)
    - [Inorder](#inorder)
    - [Preorder](#preorder)
    - [Postorder](#postorder)

## The Gist: how to visit every node once
- Traversal given any tree.
- Linear data structures (ex: array, linked lists, stacks, queues) have only one logical way to traverse through.
- However, there are multiple ways to traverse through a tree.
  - Ex: Breadth-First Search, Depth-First Search

## Breadth-First Search (BFS)
- Work across first before going down the tree.
  - i.e., visit every sibling node before moving on to children nodes.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/breadth-first-search.png" alt="Breadth-First Search" width="80%" />
</p>

### BFS Implementation
- As BFS can be used on various tree structures, graphs, etc.
- Common Principle: use a queue to keep track of the nodes that need to be visited.
- [BFS Implementation for Binary Search Tree](https://github.com/Kakamotobi/Learned/blob/main/DSA/11-Binary-Search-Trees.md)

### BFS for Binary Search Tree Flow Example
```js
// Before Iteration
  // queue: [10]
  // visited: []
// 1st Iteration
  // queue: [6, 15] // left and right of 10 is placed in queue.
  // visited: [10]
// 2nd Iteration
  // queue: [15, 3, 8] // left and right of 6 is placed in queue.
  // visited: [10, 6]
// 3rd Iteration
  // queue: [3, 8, 20] // right of 15 is placed in queue.
  // visited: [10, 6, 15]
// 4th Iteration
  // queue: [8, 20]
  // visited: [10, 6, 15, 3]
// 5th Iteration
  // queue: [20]
  // visited: [10, 6, 15, 3, 8]
// 6th Iteration
  // queue: []
  // visited: [10, 6, 15, 3, 8, 20]
```

## Depth-First Search (DFS)
- Works along each branch before backtracking.

### Three Orders of DFS
#### Inorder
- Process
  - Traverse the left subtree starting from the left and bottom most node.
  - Visit the root.
  - Traverse the right subtree.
- Inorder traveral gives nodes in non-decreasing order.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-search-inorder.png" alt="Depth-First Search Inorder" width="80%" />
</p>

#### Preorder
- Process
  - Visit the root.
  - Traverse the left subtree starting from the root.
  - Traverse the right subtree.
- Preorder traversal is used to create a copy of the tree.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-search-preorder.png" alt="Depth-First Search Preorder" width="80%" />
</p>

#### Postorder
- Process
  - Traverse the left subtree starting from the left and bottom most node.
  - Traverse the right subtree.
  - Visit the root.
- Postorder traversal is used to delete the tree.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-search-postorder.png" alt="Depth-First Search Postorder" width="80%" />
</p>














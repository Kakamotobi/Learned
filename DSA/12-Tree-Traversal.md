# Tree Traversal

## Table of Content
- [The Gist: how to visit every node once](#the-gist-how-to-visit-every-node-once)
- [BFS and DFS Big O](#bfs-and-dfs-big-o)
- [Breadth-First Search (BFS)](#breadth-first-search-bfs)
  - [BFS Implementation](#bfs-implementation)
  - [BFS for Binary Search Tree Flow Example](#bfs-for-binary-search-tree-flow-example)
- [Depth-First Search (DFS)](#depth-first-search-dfs)
  - [Preorder](#preorder)
    - [Preorder DFS Implementation](#preorder-dfs-implementation)
    - [Preorder DFS for Binary Search Tree Flow Example](#preorder-dfs-for-binary-search-tree-flow-example)
  - [Postorder](#postorder)
    - [Postorder DFS Implementation](#postorder-dfs-implementation)
    - [Postorder DFS for Binary Search Tree Flow Example](#postorder-dfs-for-binary-search-tree-flow-example)
  - [Inorder](#inorder)
    - [Inorder DFS Implementation](#inorder-dfs-implementation)
    - [Inorder DFS for Binary Search Tree Flow Example](#inorder-dfs-for-binary-search-tree-flow-example)

## The Gist: how to visit every node once
- Traversal given any tree.
- Linear data structures (ex: array, linked lists, stacks, queues) have only one logical way to traverse through.
- However, there are multiple ways to traverse through a tree.
  - Ex: Breadth-First Search, Depth-First Search

## BFS and DFS Big O
### Time Complexity
- BFS and DFS are both O(n) as each node is visited one time.
### Space Complexity
- If the tree is as wide as it can be all the way down.
  - BFS will have to store a lot of data in the queue, taking up more memory.
  - DFS will need to keep track of fewer nodes, taking less memory.
- If the tree is not wide and is lopsided to fewer branches.
  - BFS will need to keep track of fewer nodes, taking less memory.
  - DFS will have to go down and keep every level above it in memory.

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
- Work along each branch first.
- Three Orders of DFS: inorder, preorder, postorder.
### Preorder
- ***Visit the node first, then left, then right.***
- Process
  - Visit the root.
  - Traverse the left subtree starting from the root.
  - Traverse the right subtree.
- Notes
  - Can be used to "export" a tree structure so that it is easily reconstructed or copied (the root, left, right are obvious).

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-search-preorder.png" alt="Depth-First Search Preorder" width="80%" />
</p>

#### Preorder DFS Implementation
- [Preorder DFS Implementation for Binary Search Tree](https://github.com/Kakamotobi/Learned/blob/main/DSA/11-Binary-Search-Trees.md)
#### Preorder DFS for Binary Search Tree Flow Example
```js
// traverse(10) // root // visited: [10]
   // traverse(6) // 10's left // visited: [10, 6]
      // traverse(3) // 6's left // visited: [10, 6, 3]
      // traverse(8) // 6's right // visited: [10, 6, 3, 8]
   // traverse(15) // 10's right // visited: [10, 6, 3, 8, 15]
      // traverse(20) // 15's right // visited: [10, 6, 3, 8, 15, 20]
```
### Postorder
- ***Visit the node after exploring all the way down the left and the right.***
- Process
  - Traverse the left subtree starting from the left and bottom most node.
  - Traverse the right subtree.
  - Visit the root.
- Notes
  - Postorder traversal can be used to delete the tree.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-search-postorder.png" alt="Depth-First Search Postorder" width="80%" />
</p>

#### Postorder DFS Implementation
- [Postorder DFS Implementation for Binary Search Tree](https://github.com/Kakamotobi/Learned/blob/main/DSA/11-Binary-Search-Trees.md)

#### Postorder DFS for Binary Search Tree Flow Example
```js
// traverse(10) // root // visited: [3, 8, 6, 20, 15, 10] // (6)
   // traverse(6) // 10's left // visited: [3, 8, 6] // (3)
      // traverse(3) // 6's left // visited: [3] // (1) Start here, then sibling, then backtrack upwards.
      // traverse(8) // 6's right // visited: [3, 8] // (2)
   // traverse(15) // 10's right // visited: [3, 8, 6, 20, 15] // (5)
      // traverse(20) // 15's right // visited: [3, 8, 6, 20] // (4)
```
### Inorder
- ***Visit the node after exploring all the way down the left but before exploring all the way down the right.***
- Process
  - Traverse the left subtree starting from the left and bottom most node.
  - Visit the root.
  - Traverse the right subtree.
- Notes
  - Inorder traversal gives nodes in non-decreasing order.
  - Used commonly with BSTs.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-search-inorder.png" alt="Depth-First Search Inorder" width="80%" />
</p>

#### Inorder DFS Implementation
- [Inorder DFS Implementation for Binary Search Tree](https://github.com/Kakamotobi/Learned/blob/main/DSA/11-Binary-Search-Trees.md)
#### Inorder DFS for Binary Search Tree Flow Example
```js
// traverse(10) // root // visited: [3, 6, 8, 10] (4)
   // traverse(6) // 10's left // visited: [3, 6] // (2)
      // traverse(3) // 6's left // visited: [3] // (1) Start here, then parent, then sibling, then backtrack upwards.
      // traverse(8) // 6's right // visited: [3, 6, 8] // (3)
   // traverse(15) // 10's right // visited: [3, 6, 8, 10, 15] // (5)
      // traverse(20) // 15's right // visited: [3, 6, 8, 10, 15, 20] // (6)
```

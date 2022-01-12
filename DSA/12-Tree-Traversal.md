# Tree Traversal

## Table of Content
- [The Gist: how to visit every node once](#the-gist-how-to-visit-every-node-once)

## The Gist: how to visit every node once
- Traversal given any tree.
- Linear data structures (ex: array, linked lists, stacks, queues) have only one logical way to traverse through.
- However, there are multiple ways to traverse through a tree.
  - Ex: Breadth-First Search, Depth-First Search

## Breadth-First Search (BFS)
- Works across the tree first.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/breadth-first-search.png" alt="Breadth-First Search" width="80%" />
</p>

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














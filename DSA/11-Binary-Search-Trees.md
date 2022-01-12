# Binary Search Trees

## Table of Content
- [What are Trees?](#what-are-trees)
- [Binary Trees](#binary-trees)
- [Binary Search Trees](#binary-search-trees)
  - [Binary Search Tree Implementation](#binary-search-tree-implementation)

## What are Trees?
- A data structure that consists of nodes in a **parent/child** relationship.
- A node can only point to children (cannnot point to the parent or siblings).
- Trees are non-linear (multiple possible paths).
  - cf. linked lists are linear (one way path).
- There are many types of trees (ex: binary tree, binary search tree, AVL tree).
- Below: general tree example.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/tree.png" alt="Tree" width="80%" />
</p>

### Terminology
- Root: top node of a tree.
- Child: a node directly connected to another node when moving away from the root.
- Parent: the converse  notion of a child.
- Siblings: a group of nodes with the same parent.
- Leaf: a node with no children.
- Edge: the connection between one node and another.
### Some Uses for Trees
- HTML DOM.
- JSON.
- Network Routing.
- Abstract Syntax Tree.
- AI (ex: minimax tree).
- Folders in Operating Systems.

## Binary Trees
- Each node can have at most two children.

## Binary Search Trees
- A special type of binary tree.
  - The difference is that binary search trees are sorted in a particular way.
    - Every node to the left of a parent node is always less than the parent.
    - Every node to the right of a parent node is always greater than the parent.
- Binary search trees are used to store data that can be compared.
- Below: binary search tree example.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/binary-search-tree.png" alt="Tree" width="80%" />
</p>

### Binary Search Tree Big O
- Insertion - O(log n)
- Searching - O(log n)
- *As the number of nodes doubles, the number of steps required is only increased by 1.*
- However, worst case (a long branch towards one path), O(n).
  - Solution: set a new root, use a different data structure.
### Handling Duplicate Values
- Depends on what is needed.
  - If duplicates are not needed or allowed, return undefined.
  - If the number of occurences of the value is relevant, set a counter property on each node.
### Binary Search Tree Implementation
```js
class Node {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  
  // Insert value to the BST.
  insert(val) {
    const newNode = new Node(val);
    // If the BST is empty, set newNode to be the root.
    if (this.root === null) this.root = newNode;
    else {
      // Initialize currNode.
      let currNode = this.root;
      while (true) {
        // If newNode's val is the same as currNode's val.
        if (newNode.val === currNode.val) return undefined;
        // Else if newNode's val is less than currNode's val.
        else if (newNode.val < currNode.val) {
          // If there is a node to the left, move currNode to that node (restart loop).
          if (currNode.left !== null) currNode = currNode.left;
          // Else if there is no node to the left, set newNode to be its left and break loop.
          else if (currNode.left === null) {
            currNode.left = newNode;
            break;
          }
        }
        // Else if newNode's val is greater than currNode's val.
        else if (newNode.val > currNode.val) {
          // If there is a node to the right, move currNode to that node (restart loop).
          if (currNode.right !== null) currNode = currNode.right;
          // Else if there is no node to the right, set newNode to be its right and break loop.
          else if (currNode.right === null) {
            currNode.right = newNode;
            break;
          }
        }        
      }
    }
    return this;
  }
  
  // Find the node with the given value.
  find(val) {
    // If the BST is empty, return false.
    if (this.root === null) return false;
    else {
      // Initialize currNode.
      let currNode = this.root;
      while (true) {
        // If currNode's value is the same as val, return true.
        if (currNode.val === val) return currNode;
        // Else if val is less than currNode.val.
        else if (val < currNode.val) {
          // If there is a node to the left, move currNode to that node (restart loop).
          if (currNode.left !== null) currNode = currNode.left;
          // Else if there is no node to the left, return undefined.
          else if (currNode.left === null) return undefined;
        }
        // Else if val is greater than currNode.val.
        else if (val > currNode.val) {
          // If there is a node to the right, move currNode to that node (restart loop).
          if (currNode.right !== null) currNode = currNode.right;
          // Else if there is no node to the right, return undefined.
          else if (currNode.right === null) return undefined;
        }
      }      
    }
  }
  
  // Breadth-First Search
  BFS() {
    // Initialize queue with root.
    const queue = [this.root];
    // Initialize array to store the visited node values.
    const visited = [];
    // Initialize currNode.
    let currNode;
    // While there is something in the queue.
    while (queue.length !== 0) {
      // Dequeue from the queue.
      currNode = queue.shift();
      // Record currNode's val in visited array.
      visited.push(currNode.val);
      // If currNode has a left property, add it to the queue.
      if (currNode.left) queue.push(currNode.left);
      // If currNode has a right property, add it to the queue.
      if (currNode.right) queue.push(currNode.right);
    }
    return visited;
  }
}
```

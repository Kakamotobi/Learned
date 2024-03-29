# Binary Heaps

## Table of Content
- [What are Heaps?](#what-are-heaps)
- [Binary Heaps](#binary-heaps)
  - [Binary Heaps vs. Binary Search Trees](#binary-heaps-vs-binary-search-trees)
  - [Binary Heap Big O](#binary-heap-big-o)
  - [Uses of Binary Heaps](#uses-of-binary-heaps)
  - [2 Types of Binary Heaps](#2-types-of-binary-heaps)
    - [Max Binary Heap](#max-binary-heap)
    - [Min Binary Heap](#min-binary-heap)
- [Storing Binary Heaps](#storing-binary-heaps)
  - [Formulas](#formulas)
- [Max Binary Heap Implementation](#max-binary-heap-implementation)
- [Min Binary Heap Implementation Example](#min-binary-heap-implementation-example)
- [Reference](#reference)

## What are Heaps?
> A Heap is a special Tree-based data structure in which the tree is a complete binary tree. | GeeksforGeeks

- Heaps are just another category of tree data structures.
  - i.e. everything that applies to trees in general applies to heaps but has additional rules.
- There are many types of heaps.
  - Ex: binary heap, Fibonacci heap, Leonardo heap, etc.

## Binary Heaps
> A **binary heap** is a heap data structure that takes the form of a binary tree. | Wikipedia
### Binary Heaps vs Binary Search Trees
- Similar in that **each node can have at most two children**.
- However, unlike a binary search tree, **binary heaps have no order to the left or right**.
- Also, unlike a binary search tree, which can become lop-sided to one side, **binary heaps are compact and take the least amount of space possible**.
  - i.e., ***every left and right is filled before going down. All the children of each node are as full as they can be and the left children are filled out first.***
### Binary Heap Big O
- Insertion - O(log n)
- Removal - O(log n)
- Searching - O(n)
### Uses of Binary Heaps
- Binary Heaps are commonly used to implement ***Priority Queues***.
- Binary Heaps are also used with ***Graph Traversal algorithms***.
### 2 Types of Binary Heaps
#### Max Binary Heap
- **Parent nodes are always larger than child nodes.**
  - However, there are no guarantees between sibling nodes.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/max-binary-heap.png" alt="Max Binary Heap" width="80%" />
</p>

#### Min Binary Heap
- **Parent nodes are always smaller than child nodes.**
  - However, there are no guarantees between sibling nodes.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/min-binary-heap.png" alt="Min Binary Heap" width="80%" />
</p>

## Storing Binary Heaps
- Instead of creating and connecting each node manually, we can store them in an array and use their position/index to model a binary heap structure.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/storing-binary-heap-in-array.png" alt="Storing Binary Heap in Array" width="80%" />
</p>

### Formulas
#### Finding the (index of the) Children
- `leftChild = 2n + 1`
- `rightChild = 2n + 2`
#### Finding the (index of the) Parent
- `parent = Math.floor((n-1)/2)`
#### Examples
  - The children of the node at index 6 are:
    - Left Child: `6 * 2 + 1 = 13`. Index 13.
    - Right Child: `6 * 2 + 2 = 14`. Index 14.
  - The parent of the node at index 9 is:
    - `(9 - 1) / 2 = 4`. Index 4.

## Max Binary Heap Implementation
```js
class MaxBinaryHeap {
  constructor() {
    this.values = [];
  }
  
  // Insert value to the binary heap.
  // First, push the new value to the end of the array (effectively filling the heap from the left).
  // Then, keep "bubbling up" that value to its correct place.
  insert(val) {
    this.values.push(val);
    this.bubbleUp();
  }
  
  // Helper for insert (places the newly added val, which is at the end of the array, to its right place).
  bubbleUp() {
    // Initialize variable to keep track of where the newly inserted value is in the array.
    let idx = this.values.length - 1;
    // While idx is a valid index.
    while (idx > 0) {
      // Find index of parent.
      let parentIdx = Math.floor((idx - 1)/2);
      // If val is less than its parent, the value is in its correct place.
      if (this.values[idx] < this.values[parentIdx]) break;
      else {
        // Swap places.
        [this.values[idx], this.values[parentIdx]] = [this.values[parentIdx], this.values[idx]];
        // Update idx.
        idx = parentIdx;
      }
    }
  }
  
  // Remove and return the root from the binary heap.
  // First, swap the root and the last value in the heap.
  // Then, keep "sinking down" that last value to its correct place.
  // Note: when removing an element from a priority queue based on its priority level, the highest priority would be the root from the heap. Therefore, remove the root.
  extractMax() {
    // Swap the root with the last value.
    [this.values[0], this.values[this.values.length-1]] = [this.values[this.values.length-1], this.values[0]];
    // Remove the original root.
    const removeTarget = this.values.pop();
    // Readjust the binary heap ("sink down" the new root to its correct position).
    this.sinkDown();
    // Return the original root.
    return removeTarget;
  }
  
  // Helper for extractMax (readjusts the binary heap after removing the root).
  sinkDown() {
    // Initialize idx to the root's index.
    let idx = 0;
    while (true) {
      // Find the index of the children.
      let leftChildIdx = 2 * idx + 1;
      let rightChildIdx = 2 * idx + 2;
      // Grab the corresponding values of the element and children.
      let element = this.values[idx];
      let leftChild = this.values[leftChildIdx];
      let rightChild = this.values[rightChildIdx];
      // If leftChild is greater than both the rightChild and element OR leftChild is valid but rightChild is invalid and leftChild is greater than the element.
      if ((leftChild > rightChild && leftChild > element) || (leftChild && !rightChild && leftChild > element)) {
        [this.values[leftChildIdx], this.values[idx]] = [this.values[idx], this.values[leftChildIdx]];
        idx = leftChildIdx;
      }
      // Else if rightChild is greater than both the leftChild and element OR rightChild is valid but leftChild is invalid and rightChild is greater than the element.
      else if ((rightChild > leftChild && rightChild > element) || (rightChild && !leftChild && rightChild > element)) {
        [this.values[rightChildIdx], this.values[idx]] = [this.values[idx], this.values[rightChildIdx]];
        idx = rightChildIdx;
      }
      // Else, break.
      else break;
    }
  }
}
```

## Min Binary Heap Implementation Example
- [Priority Queue Implementation](https://github.com/Kakamotobi/Learned/blob/main/DSA/14-Priority-Queue.md#priority-queue-implementation-using-min-binary-heap)

## Reference
[Heap Data Structure - GeeksforGeeks](https://www.geeksforgeeks.org/heap-data-structure/)  
[Binary heap - Wikipedia](https://en.wikipedia.org/wiki/Binary_heap)

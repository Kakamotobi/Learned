# Priority Queue

## Table of Contents
- [What are Priority Queues?](#what-is-are-priority-queues)
- [Simple Priority Queue Implementation Using an Array](#priority-queue-implementation-using-an-array)
- [Priority Queue Implementation Using Min Binary Heap](#priority-queue-implementation-using-min-binary-heap)

## What are Priority Queues?
- **A data structure where each element has a priority. Elements with higher priorities are served before elements with lower priorities.**
  - Ex: computer processes.
- *Priority Queues are an abstract concept separate from heaps. Other data structures such as arrays can be used to implement a priority queue (although not as efficient as using binary heap).*

## Simple Priority Queue Implementation Using an Array
```js
class PriorityQueue {
  constructor() {
    this.nodes = [];
  }
  
  enqueue(val, priority) {
    // Assign a priority for each val.
    this.nodes.push({val, priority});
    // Sort the values based on priority.
    this.sort();
  }
  
  dequeue() {
    return this.nodes.shift();
  }
  
  // O(n * log n)
  sort() {
    this.nodes.sort((a, b) => a.priority - b.priority);
  }
}
```

## Priority Queue Implementation Using Min Binary Heap
```js
// Each Node has a value and a priority (which is used to build the heap).
class Node {
  constructor(val, priority) {
    this.val = val;
    this.priority = priority;
  }
}
```
```js
class PriorityQueue {
  constructor() {
    this.nodes = [];
  }
  
  // Add to priority queue.
  enqueue(val, priority) {
    const newNode = new Node(val, priority);
    this.nodes.push(newNode);
    this.bubbleUp();
  }
  
  // Helper for enqueue (places the new node in its right place based on its priority).
  bubbleUp() {
    // Initialize variable to keep track of where the newly inserted value is in the array.
    let idx = this.nodes.length - 1;
    while (idx > 0) {
      let parentIdx = Math.floor((idx - 1) / 2);
      // If priority value is greater than its parent's, break.
      if (this.nodes[idx].priority > this.nodes[parentIdx].priority) break;
      else {
        // Swap places.
        [this.nodes[idx], this.nodes[parentIdx]] = [this.nodes[parentIdx], this.nodes[idx]];
        // Update idx.
        idx = parentIdx;
      }
    }
  }
  
  // Removes and returns the root.
  dequeue() {
    // Swap the root with the last value.
    [this.nodes[0], this.nodes[this.nodes.length-1]] = [this.nodes[this.nodes.length-1], this.nodes[0]];
    // Remove the original root.
    const removeTarget = this.nodes.pop();
    // Readjust the binary heap ("sink down" the new root to its correct position).
    this.sinkDown();
    // Return the original root.
    return removeTarget;
  }
  
  // Helper for dequeue (rearranges the binary heap using priority).
  sinkDown() {
    // Initialize idx to the root's index.
    let idx = 0;
    while(true) {
      // Find the index of the children.
      let leftChildIdx = 2 * idx + 1;
      let rightChildIdx = 2 * idx + 2;
      // Grab the corresponding values of the element and children.
      let element = this.nodes[idx];
      let leftChild = this.nodes[leftChildIdx];
      let rightChild = this.nodes[rightChildIdx];
      // If leftChild's priority value is less than that of rightChild's and leftChild's priority value is less than that of element's priority OR
      // leftChild is valid and rightChild is invalid and leftChild's priority value is less than that of element's priority.
      if ((leftChild?.priority < rightChild?.priority && leftChild?.priority < element.priority) || (leftChild && !rightChild && leftChild?.priority < element.priority)) {
        [this.nodes[leftChildIdx], this.nodes[idx]] = [this.nodes[idx], this.nodes[leftChildIdx]];
        idx = leftChildIdx;
      }
      // Else if rightChild's priority value is less than that of leftChild's and rightChild's priority value is less than that of element's priority OR
      // rightChild is valid and leftChild is invalid and rightChild's priority value is less than that of element's priority.
      else if ((rightChild?.priority < leftChild?.priority && rightChild?.priority < element.priority) || (rightChild && !leftChild && rightChild?.priority < element.priority)) {
        [this.nodes[rightChildIdx], this.nodes[idx]] = [this.nodes[idx], this.nodes[rightChildIdx]];
        idx = rightChildIdx;
      }
      else break;
    }
  }
}
```


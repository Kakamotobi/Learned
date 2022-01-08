# Doubly Linked List

## Table of Content
- [What are Doubly Linked Lists?](#what-are-doubly-linked-lists)
- [Doubly Linked List Big O](#doubly-linked-list-big-o)
- [Doubly Linked List Implementation](#doubly-linked-list-implementation)

## What are Doubly Linked Lists?
- Almost identical to Singly Linked Lists, except that every node has another pointer to the previous node.
- The **head** also points to null.
- The **tail** also points to its previous node.
- Doubly Linked Lists are more flexible but consume more memory than Singly Linked Lists.
- Use Case Example
  - Browser history (back, forward).

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/doubly-linked-list.png" alt="Singly Linked Lists" />
</p>

## Doubly Linked List Big O
- Insertion - O(1)
  - Finding the index to insert at - O(1) or O(n)
  - Performing the insertion - O(1)
- Removal - O(1)
  - Finding the index to insert at - O(1) or O(n)
  - Performing the insertion - O(1)
- Searching - O(n)
  - Technically, O(n/2), which is simplified to O(n).
- Access - O(n)

## Doubly Linked List Implementation
```js
class Node {
  constructor(val) {
    this.val = val;
    this.prev = null;
    this.next = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null
    this.length = 0;
  }
  
  // Add a node to the end of the linked list.
  push(val) {
    // Create a new node using val.
    const newNode = new Node(val);
    // If list is empty.
    if (this.length === 0) {
      // Set the head and tail to be newNode;
      this.head = newNode;
      this.tail = newNode;
    } else {
      // Set tail's next to that newNode.
      this.tail.next = newNode;
      // Set newNode's prev to tail.
      newNode.prev = this.tail;
      // Update the tail.
      this.tail = newNode;
    }
    // Increment length;
    this.length++;
    return this;
  }
  
  // Remove a node from the end of the linked list.
  pop() {
    // If list is empty, return undefined.
    if (this.length === 0) return undefined;
    // Initialize popTarget to be the current tail.
    const popTarget = this.tail;
    // If the length is 1, set the head and tail to be null.
    if (this.length === 1) {
      this.head = null;
      this.tail = null;
    } else {
      // Update the tail.
      this.tail = popTarget.prev;
      // Set the new tail's next to be null.
      this.tail.next = null;
      // Set popTarget's prev to be null.
      popTarget.prev = null;
    }
    // Decrement length.
    this.length--;
    return popTarget;
  }
  
  // Remove a node from the beginning of the linked list.
  shift() {
    // If list is empty, return undefined.
    if (this.length === 0) return undefined;
    // Initialize shiftTarget to be the current head.
    const shiftTarget = this.head;
    // If the length is 1, set the head and tail to be null.
    if (this.length === 1) {
      this.head = null;
      this.tail = null;
    } else {
      // Update the head.
      this.head = shiftTarget.next;
      // Set the new head's prev to be null.
      this.head.prev = null;
      // Set shiftTarget's next to be null.
      shiftTarget.next = null;
    }
    // Decrement length.
    this.length--;
    return shiftTarget;
  }
  
  // Add a node to the beginning of the linked list.
  unshift(val) {
    // Create a new node using val.
    const newNode = new Node(val);
    // If list is empty.
    if (this.length === 0) {
      // Set the head and tail to be newNode;
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.next = this.head;
      this.head.prev = newNode;
      this.head = newNode;
    }
    // Increment length.
    this.length++;
    return this;
  }
  
  // Retrieve a node by its position (0-based) in the linked list.
  get(idx) {
    // If idx is invalid, return null.
    if (idx < 0 || idx >= this.length) return null;
    // Initialize idxTracker and currNode.
    let idxTracker;
    let currNode;
    // If the index is less than or equal to half the length of the list.
    if (idx <= this.length/2) {
      // Set idxTracker to 0.
      idxTracker = 0;
      // Set currNode to head.
      currNode = this.head;
      // While idx hasn't been reached yet.
      while(idxTracker !== idx) {
        // Move currNode.
        currNode = currNode.next;
        // Increment idxTracker.
        idxTracker++;
      }
    }
    // Else if the index is greater than half the length of the list.
    else if (idx > this.length/2) {
      // Set idxTracker to last index.
      idxTracker = this.length - 1;
      // Set currNode to tail.
      currNode = this.tail;
      // While idx hasn't been reached yet.
      while (idxTracker !== idx) {
        // Move currNode.
        currNode = currNode.prev;
        // Decrement idxTracker.
        idxTracker--;
      }
    }
    return currNode;
  }
  
  // Change the value of a node based on it's position in the linked list.
  set(idx, val) {
    // Get the specific node.
    const targetNode = this.get(idx);
    // If node is found, set the target node's value to be the new val.
    if (targetNode) {
      targetNode.val = val;
      return true;
    }
    return false;
  }
  
  // Add a node to a specific position in the linked list.
  insert(idx, val) {
    // If idx is invalid, return false.
    if (idx < 0 || idx > this.length) return false;
    // If idx is equal to the length, push new node to the end of the list.
    if (idx === this.length) return !!this.push(val);
    // If idx is 0, unshift new node to the start of the list.
    if (idx === 0) return !!this.unshift(val);
    // Else idx is somewhere in the middle.
    else {
      const newNode = new Node(val);
      // Get the previous node of idx.
      const prevNode = this.get(idx-1);
      // Get the original node at the idx.
      const originalNode = prevNode.next;
      // Set the next and prev properties on the correct nodes to link everything together.
      prevNode.next = newNode;
      newNode.prev = prevNode;
      newNode.next = originalNode;
      originalNode.prev = newNode;
      // Increment length.
      this.length++;
      return true;
    }
  }
  
  // Remove a node at a specific position in the linked list.
  remove(idx) {
    // If idx is invalid, return undefined.
    if (idx < 0 || idx >= this.length) return undefined;
    // If idx is 0, shift.
    if (idx === 0) return this.shift();
    // If idx is the last node, pop.
    if (idx === this.length - 1) return this.pop();
    // Else idx is somewhere in the middle.
    else {
      // Get the node at the target idx.
      const removeTarget = this.get(idx);
      // Initialize prevNode and nextNode.
      const prevNode = removeTarget.prev;
      const nextNode = removeTarget.next;
      // Set the next and prev properties on the correct nodes.
      prevNode.next = nextNode;
      nextNode.prev = prevNode;
      // Set removeTarget's prev and next to null.
      removeTarget.prev = null;
      removeTarget.next = null;
      // Decrement length.
      this.length--;
      return removeTarget;
    }
  }
  
  // Reverse the linked list in place.
  reverse() {
    // Initialize newNextNode and currNode.
    let newNextNode = null;
    let currNode = this.head;
    // While currNode hasn't reached the end yet.
    while (currNode !== null) {
      // Preserve currNode's previous node.
      newNextNode = currNode.prev;
      // Reverse currNode's next and prev.
      currNode.prev = currNode.next;
      currNode.next = newNextNode;
      // Move currNode.
      currNode = currNode.prev;
    }
    // Swap the head and tail.
    [this.head, this.tail] = [this.tail, this.head];
    return this;
  }
}
```
```js
// Different approach to reverse.
reverse() {
  let currNode = this.head;
  [this.head, this.tail] = [this.tail, this.head];
  let nextNode;
  for (let i = 0; i < this.length; i++) {
    nextNode = currNode.next;
    currNode.next = currNode.prev;
    currNode.prev = nextNode;
    currNode = nextNode;
  }
  return this;
}
```











# Doubly Linked List

## Table of Content
- [What are Doubly Linked Lists?](#what-are-doubly-linked-lists)
- [Doubly Linked List Big O](#doubly-linked-list-big-o)
- [Doubly Linked List Implementation](#doubly-linked-list-implementation)

## What are Doubly Linked Lists?
- Almost identical to Singly Linked Lists, except that every node has another pointer to the previous node.
- The **head** also points to null.
- The **tail** also points to its previous node.
- Doubly Linked Lists are more flexibly but consume more memory than Singly Linked Lists.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/doubly-linked-list.png" alt="Singly Linked Lists" />
</p>

## Doubly Linked List Big O

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
    // Increment the length;
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
    // Decrement the length.
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
    // Decrement the length.
    this.length--;
    return shiftTarget;
  }
}
```












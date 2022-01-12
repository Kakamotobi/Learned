# Singly Linked Lists

## Table of Contents
- [What are Singly Linked Lists](#what-are-singly-linked-lists)
  - [Singly Linked Lists vs. Arrays](#singly-linked-lists-vs-arrays)
- [Singly Linked List Big O](#singly-linked-list-big-o)
- [Singly Linked List Implementation](#singly-linked-list-implementation)

## What are Singly Linked Lists
- An ordered list of data that contains a **head**, **tail**, and **length** property.
  - Head: the beginning of the linked list.
  - Tail: the end of the linked list.
- Linked lists consist of nodes, and each **node**(element) has a **value** and a **pointer** to another node or null.
  - If you need to access a node/element, you need to start from the beginning.
- ***Singly*** refers to the fact that each node is only connected one directionally to the next node.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/singly-linked-list.png" alt="Singly Linked Lists" width="80%" />
</p>

### Singly Linked Lists vs. Arrays
- Linked lists
  - Do not have indices.
  - Connected via nodes with a *next* pointer.
  - Random access is not allowed.
- Arrays
  - Indexed in order.
  - Insertion and deletion can be expensive (reindexing required).
    - *One of the biggest reason to use a linked list is if you need to care for insertion/deletion of data and random access is not needed.*
  - Can quickly be accessed at a specific index.
- *Singly linked lists are an excellent alternative to arrays when insertion and deletion at the beginning are frequently required and random access is not needed.*

## Singly Linked List Big O
- Insertion - O(1)
  - Finding the index to insert at - O(1) or O(n)
  - Performing the insertion - O(1)
- Removal - O(1)
  - Finding the index to remove - O(1) or O(n)
  - Performing the removal - O(1)
- Searching - O(n)
- Access - O(n)

## Singly Linked List Implementation
```js
class Node {
  constructor(val) {
    this.val = val; // the element.
    this.next = null; // reference to the next node.
  }
}

class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
  
  // Add a node to the end of the linked list.
  push(val) {
    // Create a new node using val.
    const newNode = new Node(val);
    // If head is null (meaning, list is empty).
    if (!this.head) {
      // Set the head and tail to be the newly created node.
      this.head = newNode;
      this.tail = newNode;
    }
    // Else (list is not empty).
    else {
      // Set the next property on the current tail to be the newly created node.
      this.tail.next = newNode;
      // Set the tail property on the list to be the newly created node.
      this.tail = newNode;
    }
    // Increment the length by one.
    this.length++;
    // Return the singly linked list.
    return this;
  }
  
  // Remove a node from the end of the linked list.
  pop() {
    // If list is empty, return undefined.
    if (!this.length) return undefined;
    // Else if list has only one node.
    else if (this.length === 1) {
      const popTarget = this.head;
      this.head = null;
      this.tail = null;
      this.length--;
      return popTarget;
    }
    else {      
      // Initialize pointer to identify the 2nd to last element (new tail).
      let newTail = this.head;
      // Initialize popTarget to be the current tail.
      const popTarget = this.tail;
      // Loop until newTail reaches the 2nd to last element.
      while (newTail.next !== popTarget) {
        // Update newTail.
        newTail = newTail.next;
      }
      // Set the newTail's next property to be null.
      newTail.next = null;
      // Update this.tail to be newTail.
      this.tail = newTail;
      // Decrement the length of the list by 1.
      this.length--;
      return popTarget;
    }
  // Garbage Collection. After a pop there remains no references to the removed tail. So the JS engine safely deletes it from memory.
  }
  
  // Remove a node from the beginning of the linked list.
  shift() {
    if (this.length === 0) return undefined;
    // Store the current head property in a variable (to return at the end).
    const shiftTarget = this.head;
    // Set the head property to be the current head's next property.
    this.head = shiftTarget.next;
    // Decrement length.
    this.length--;
    // If list is empty after shift, set the tail to null.
    if (this.length === 0) this.tail = null;
    return shiftTarget;
  }
  
  // Add a new node to the beginning of the linked list.
  unshift(val) {
    const newNode = new Node(val);
    if (!this.length) {
      this.head = newNode;
      this.tail = newNode;
    }
    else {
      // Set the next property for the new node to be the current head.
      newNode.next = this.head;
      // Update the current head to be the new node.
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
    // Initialize counter.
    let counter = 0;
    // Initialize target to be the first node.
    let target = this.head;
    // Loop until index is reached.
    while (counter !== idx) {
      // Update target to be the next node.
      target = target.next;
      // Increment counter.
      counter++;
    }
    return target;
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
      // Get the previous node.
      const prevNode = this.get(idx-1);
      // Get the original node at the target idx.
      const originalNode = prevNode.next;
      // Set the next property on prevNode to be newNode.
      prevNode.next = newNode;
      // Set the next property on newNode to be originalNode.
      newNode.next = originalNode;
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
      // Get the previous node. 
      const prevNode = this.get(idx-1);
      // Store removeTarget.
      const removeTarget = prevNode.next;
      // Set the next property on prevNode to be the next of the next node.
      prevNode.next = removeTarget.next;
      // Decrement length.
      this.length--;
      return removeTarget;
    }
  }
  
  // Reverse the linked list in place.
  reverse() {
    // Swap the head and tail.
    [this.head, this.tail] = [this.tail, this.head];
    // Initialize currNode, prevNode, nextNode.
    let currNode = this.tail;
    let prevNode = null;
    let nextNode = null;
    // Loop through the list.
    for (let i = 0; i < this.length; i++) {
      // Preserve the reference to the nextNode before effectively reversing currNode's link direction.
      nextNode = currNode.next;
      // Reverse the link direction.
      currNode.next = prevNode;
      // Move on.
      prevNode = currNode;
      currNode = nextNode;
    }
    return this;
  }
}
```





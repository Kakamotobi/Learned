# Singly Linked Lists

## Table of Contents
- [What are Singly Linked Lists](#what-are-singly-linked-lists)
- [Implementing Singly Linked List](#implementing-singly-linked-list)

## What are Singly Linked Lists
- An ordered list of data that contains a **head**, **tail**, and **length** property.
  - Head: the beginning of the linked list.
  - Tail: the end of the linked list.
- Linked lists consist of nodes, and each **node**(element) has a **value** and a **pointer** to another node or null.
  - If you need to access a node/element, you need to start from the beginning.
- ***Singly*** refers to the fact that each node is only connected one directionally to the next node.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/singly-linked-list.png"/ alt="Singly Linked Lists">
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

## Implementing Singly Linked List
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
      let popTarget = this.head;
      this.head = null;
      this.tail = null;
      this.length--;
      return popTarget;
    }
    else {      
      // Initialize pointer to identify the 2nd to last element (new tail).
      let newTail = this.head;
      // Initialize popTarget to be the current tail.
      let popTarget = this.tail;
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
      // Return the value of the node removed.
      return popTarget;
    }
  // Garbage Collection. After a pop there remains no references to the removed tail. So the JS engine safely deletes it from memory.
  }
}
```





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
  constructor(val, next = null) {
    this.val = val;
    this.next = next;
  }
}

export class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }

  isEmpty() {
    return this.length === 0 ? true : false;
  }

  size() {
    return this.length;
  }

  getByIdx(idx) {
    if (idx < 0 || idx >= this.length) return undefined;
    let counter = 0;
    let currNode = this.head;
    while (counter !== idx) {
      currNode = currNode.next;
      counter++;
    }
    return currNode;
  }

  getById(valId) {
    let currNode = this.head;
    while (currNode !== null) {
      if (currNode.val.id === valId) return currNode;
      currNode = currNode.next;
    }
    return undefined;
  }

  set(idx, val) {
    if (idx < 0 || idx >= this.length) return undefined;
    const target = this.get(idx);
    if (target) {
      target.val = val;
      return true;
    }
    return false;
  }

  addToStart(val) {
    const newNode = new Node(val);
    if (this.length === 0) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.next = this.head;
      this.head = newNode;
    }
    this.length++;
    return this;
  }

  addToEnd(val) {
    const newNode = new Node(val);
    if (this.length === 0) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      this.tail.next = newNode;
      this.tail = newNode;
    }
    this.length++;
    return this;
  }

  insertAtIdx(idx, val) {
    if (idx < 0) return false;
    if (idx === 0) return !!this.addToStart(val);
    if (idx === this.length || idx >= this.length) return !!this.addToEnd(val);
    else {
      const newNode = new Node(val);
      const prevNode = this.getByIdx(idx - 1);
      const nodeAtIdx = prevNode.next;
      prevNode.next = newNode;
      newNode.next = nodeAtIdx;
      this.length++;
      return true;
    }
  }

  removeFromStart() {
    if (this.length === 0) return undefined;
    const target = this.head;
    this.head = target.next;
    target.next = null;
    this.length--;
    if (this.length === 0) this.tail = null;
    return target;
  }

  removeFromEnd() {
    if (this.length === 0) return undefined;
    else if (this.length === 1) {
      const target = this.tail;
      this.head = null;
      this.tail = null;
      return target;
    } else {
      const target = this.tail;
      const newTail = this.getByIdx(this.length - 2);
      newTail.next = null;
      this.tail = newTail;
      this.length--;
      return target;
    }
  }

  removeByIdx(idx) {
    if (idx < 0 || idx >= this.length) return undefined;
    if (idx === 0) return this.removeFromStart();
    else if (idx === this.length - 1) return this.removeFromEnd();
    else {
      const prevTargetNode = this.getByIdx(idx - 1);
      const target = prevTargetNode.next;
      prevTargetNode.next = target.next;
      target.next = null;
      this.length--;
      return target;
    }
  }

  removeById(valId) {
    let prevNode;
    let currNode = this.head;
    while (currNode !== null) {
      if (currNode.val.id === valId) {
        if (currNode === this.head) return this.removeFromStart();
        else if (currNode === this.tail) return this.removeFromEnd();
        else {
          prevNode.next = currNode.next;
          currNode.next = null;
          this.length--;
          return currNode;
        }
      }
      prevNode = currNode;
      currNode = currNode.next;
    }

    return undefined;
  }

  // Reverse the linked list in place.
  reverse() {
    [this.head, this.tail] = [this.tail, this.head];
    let currNode = this.tail;
    let prevNode = null;
    let nextNode = null;
    for (let i = 0; i < this.length; i++) {
      nextNode = currNode.next;
      currNode.next = prevNode;
      prevNode = currNode;
      currNode = nextNode;
    }
    return this;
  }

  // Rotate all the nodes in the linked list by num.
  rotate(num) {
    num = num % this.length;
    if (num < 0) num = this.length + num;
    if (num === 0) return this;
    else {
      const newTail = this.get(num - 1);
      const newHead = newTail.next;
      newTail.next = null;
      this.tail.next = this.head;
      this.head = newHead;
      this.tail = newTail;
      return this;
    }
  }

  // Return the kth to last node.
  kthToLast(k) {
    let targetNode = this.head;
    for (let i = 0; i < this.length - k; i++) {
      targetNode = targetNode.next;
    }
    return targetNode;
  }

  // Check if linked list is a palindrome.
  isPalindrome() {
    // Compute linked list values into a string.
    let str = "";
    let currNode = this.head;
    while (currNode !== null) {
      str += currNode.val;
      currNode = currNode.next;
    }

    // Reverse str.
    let reversed = "";
    for (let i = str.length - 1; i >= 0; i--) {
      reversed += str[i];
    }

    return str === reversed;
  }
}
```

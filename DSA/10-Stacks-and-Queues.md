# Stacks and Queues

## Table of Content
- [Stacks](#stacks)
  - [What are Stacks?](#what-are-stacks)
  - [Stack Big O](#stack-big-o)
  - [Stack Implementations](#stack-implementations)
    - [Stack Using Array - `push` and `pop`](#stack-using-array---push-and-pop)
    - [Stack Using Singly Linked List](#stack-using-singly-linked-list)
- [Queues](#queues)
  - [What are Queues?](#what-are-queues)
  - [Queue Big O](#queue-big-o)
  - [Queue Implementation](#queue-implementation)
    - [Queue Using Array - `push` and `shift` or `unshift` and `pop`](#queue-using-array---push-and-shift-or-unshift-and-pop)
    - [Queue Using Singly Linked List](#queue-using-singly-linked-list)

## Stacks
### What are Stacks?
- **A FILO/LIFO abstract data structure.**
- Some use cases:
  - Manage function invocations (call stack).
  - Undo/Redo.
  - Routing (the history object in the browser) is treated like a stack.
### Stack Big O
- Insertion - O(1)
- Removal - O(1)
- Searching - O(n)
- Access - O(n)
### Stack Implementations
#### Stack Using Array - `push` and `pop`
- No reindexing required (O(1)).
```js
let stack = [];
stack.push("google");
stack.push("instagram");
stack.push("youtube");
stack; // ["google", "instagram", "youtube"]
stack.pop(); // "youtube"
stack.pop(); // "instagram"
stack.push("amazon");
stack.pop(); // "amazon"
stack.pop(); // "google"
```
#### Stack Using Singly Linked List
```js
class Node {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.first = null;
    this.last = null;
    this.size = 0;
  }
  
  // Add to the beginning of the linked list (for TC: O(1)).
  push(val) {
    const newNode = new Node(val);
    // If the stack is empty, set the first and last to be newNode.
    if (this.size === 0) {
      this.first = newNode;
      this.last = newNode;
    }
    else {
      // Preserve current first.
      let oldFirst = this.first;
      // Update first to be newNode.
      this.first = newNode;
      // Set newNode's next to be the old first.
      newNode.next = oldFirst;
    }
    return ++this.size;
  }
  
  // Remove from the beginning of the linked list (for TC: O(1)).
  pop() {
    if (this.size === 0) return null;
    // Initialize popTarget to be the current first.
    const popTarget = this.first;
    // If the size is 1, set the first and last to be null.
    if (this.size === 1) {
      this.first = null;
      this.last = null;
    }
    else {
      // Update the first.
      this.first = popTarget.next;
    }
    this.size--;
    return popTarget.val;
  }
  // Since popTarget's value is being returned and not the node itself, there will be no possible reference remaining to the node after being popped.
  // Therefore, the garbage collector collects popTarget and it's next property to the new first.
}
```
#### Example
```js
let stack = new Stack();

stack.push(1); // 1
stack.push(2); // 2
stack.push(3); // 3
stack.push(4); // 4
stack.push(5); // 5

// Consider "last" and "first" to represent the order of being popped.
      last                first
null <- 1 <- 2 <- 3 <- 4 <- 5

stack.pop() // 5
stack.pop() // 4
stack.pop() // 3
stack.pop() // 2
stack.pop() // 1
stack.pop() // null
```

## Queues
### What are Queues?
- **A FIFO/LILO abstract data structure.**
- Some use cases:
  - Waiting to join a game server.
  - Background tasks.
  - Uploading resources.
  - Printing / Task Processing.
### Queue Big O
- Insertion - O(1)
- Removal - O(1)
- Searching - O(n)
- Access - O(n)
### Queue Implementation
#### Queue Using Array - `push` and `shift` or `unshift` and `pop`
- Reindexing required (O(n)).
```js
let queue = [];
queue.push("first");
queue.push("second");
queue.push("third");
queue; ["first", "second", "third"]
queue.shift(); // "first"
queue.shift(); // "second"
queue.shift(); // "third"
```
#### Queue Using Singly Linked List
```js
class Node {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.first = null;
    this.last = null;
    this.size = 0;
  }
  
  // Add to the end of the linked list.
  enqueue(val) {
    const newNode = new Node(val);
    // If the queue is empty, set the first and last to be newNode.
    if (this.size === 0) {
      this.first = newNode;
      this.last = newNode;
    }
    else {
      // Set the current last's next to be newNode.
      this.last.next = newNode;
      // Update the last to be newNode.
      this.last = newNode;
    }
    return ++this.size;
  }
  
  // Remove from the beginning of the linked list.
  dequeue() {
    // If the queue is empty, return null.
    if (this.size === 0) return null;
    // Initialize dequeueTarget to be the current first.
    const dequeueTarget = this.first;
    // If the size is 1, set the first and last to be null.
    if (this.size === 1) {
      this.first = null;
      this.last = null;
    }
    else {
      this.first = this.first.next;
    }
    this.size--;
    return dequeueTarget.val;
  }
}
```
#### Example
```js
let queue = new Queue();

queue.enqueue(1); // 1
queue.enqueue(2); // 2
queue.enqueue(3); // 3
queue.enqueue(4); // 4
queue.enqueue(5); // 5

// Consider "first" and "last" to represent the order of being dequeued.
first               last
  1 -> 2 -> 3 -> 4 -> 5 -> null

queue.dequeue(); // 1
queue.dequeue(); // 2
queue.dequeue(); // 3
queue.dequeue(); // 4
queue.dequeue(); // 5
queue.dequeue(); // null
```

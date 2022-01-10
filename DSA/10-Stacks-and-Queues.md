# Stacks and Queues

## Table of Content
- [Stacks](#stacks)
  - [What are Stacks?](#what-are-stacks)
  - [Stack Big O](#stack-big-o)
  - [Stack Implementation](#stack-implementation)
- [Queues](#queues)
  - [What are Queues?](#what-are-queues)
  - [Queue Big O](#queue-big-o)
  - [Queue Implementation](#queue-implementation)

## Stacks
### What are Stacks?
- **A FILO/LIFO abstract data structure.**
- Some use cases:
  - Manage function invocations (call stack).
  - Undo/Redo
  - Routing (the history object in the browser) is treated like a stack.
### Stack Big O
- Insertion - O(1)
- Removal - O(1)
- Searching - O(n)
- Access - O(n)
### Stack Implementations
#### Using Array - `push` and `pop`
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
#### Using Singly Linked List
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
### Queue Big O
### Queues Implementation

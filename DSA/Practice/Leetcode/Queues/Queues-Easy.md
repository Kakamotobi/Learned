# Queues - Easy

## 232. Implement Queue using Stacks
- Description
  - Implement a queue using only two stacks.
- Instructions
  - `push(x)` pushes x to the back of the queue.
  - `pop()` removes the element from the front of the queue and returns it.
  - `peek()` returns the element at the front of the queue.
  - `empty()` returns `true` if the queue is empty, otherwise `false`.
- Constraints
  - Must only use standard operations of a stack (`push to top`, `peek/pop from top`, `size`, and `is empty`).
### Solution
```js
const MyQueue = function() {
  this.stack1 = [];
  this.stack2 = [];
}

MyQueue.prototype.push = function(x) {
  this.stack1.push(x);
}

MyQueue.prototype.pop = function () {
  while (this.stack1.length !== 0) {
    this.stack2.push(this.stack1.pop());
  }
  const popTarget = this.stack2.pop();
  while (this.stack2.length !== 0) {
    this.stack1.push(this.stack2.pop());
  }
  return popTarget;
}

MyQueue.prototype.peek = function () {
  while (this.stack1.length !== 0) {
    this.stack2.push(this.stack1.pop());
  }
  const peekTarget = this.stack2.pop();
  this.stack2.push(peekTarget);
  while (this.stack2.length !== 0) {
    this.stack1.push(this.stack2.pop());
  }
  return peekTarget;
}

MyQueue.prototype.empty = function () {
  return this.stack1.length === 0 ? true : false;
}
```

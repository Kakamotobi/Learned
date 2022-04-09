# Chapter 3 - Stacks and Queues

## 3.2 Stack Min
- Design a stack which, in addition to `push()` and `pop()`, has a function `min()` which returns the minimum element. All functions should operate in O(1) time.
### Solutions
#### Approach 1
- Have a variable in the stack class that keeps track of the minimum value.
- However, if the minimum value is popped, we would have to search through the whole stack (O(n)) to find the new minimum value.
- Hence, very exhaustive process.
#### Approach 2
- Make each node in the stack keep record of the minimum element below it (inclusive).
- Therefore, upon pushing a node, store its value and the minimum value to that point as well.

## 3.4 Queue via Stacks
- Implement a queue using two stacks.
### Solution
```js
class MyQueue {
  constructor() {
    this.stack1 = [];
    this.stack2 = [];
  }
  
  add(val) {
    this.stack1.push(val);
  }
  
  remove() {
    if (stack2.length === 0) {
      while (stack1.length > 0) {
        stack2.push(stack1.pop());
      }
    }
    
    return stack2.pop();
  }
}
```

## 3.5 Sort Stack
- Sort a stack so that the smallest items are on the top.
- Allowed to use additional temporary stack, but not allowed to copy the elements into any other data structure (ex: array).
- The stack supports `push()`, `pop()`, `peek()`, and `isEmpty()`.
### Solution
- Two stacks `main` and `helper`.
- Loop through `main`.
  - Pop from `main` and store in a temporary variable `temp`.
  - Loop through `helper`.
    - If the value is larger than `temp`, pop and push to `main` until `temp` finds its right place in order (larger items on the top of `helper`). 
- Pop all values from `helper` and push to `main`.

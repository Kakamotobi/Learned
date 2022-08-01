# Linked List

## 1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
```js
// TC: O(n), SC: O(1)

const reverseList = (head) => {
  let currNode = head;
  let prevNode = null;
  let nextNode = null;
  
  while (currNode !== null) {
    nextNode = currNode.next;
    currNode.next = prevNode;
    // Move on.
    prevNode = currNode;
    currNode = nextNode;
  }
  
  return prevNode;
}
```

## 2. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
- Floyd's Slow/Fast Pointers
```js
// TC: O(n), SC: O(1)

const hasCycle = (head) => {
  let slow = head;
  let fast = head;
  
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }
  
  return false;
}
```

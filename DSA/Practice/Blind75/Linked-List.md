# Linked List

## 1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
```js
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

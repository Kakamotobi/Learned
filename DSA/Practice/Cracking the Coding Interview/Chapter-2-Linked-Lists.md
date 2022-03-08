# Chapter 2 - Linked Lists

## 2.2 Return Kth to Last
- Input: `head` of a singly linked list and integer `k`.
- Output: return the `k`th to last element of a singly linked list.
### Example
```js
// Inputs
  // head = 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7
  // k = 3
// Output: 5
```
### Solutions
#### Approach 1 - O(n)
- Find out the length of the linked list.
- Find and return the `(length - k)`th node.
```js
const kthToLast = (head, k) => {
  let length = 0;
  let currNode = head;
  while (currNode !== null) {
    currNode = currNode.next;
    length++;
  }
  
  currNode = head;
  for (let i = 0; i < length - k; i++) {
    currNode = currNode.next;
  }
  return currNode;
}
```
#### Approach 2 - O(n - k)
- Initialize two pointers so that they are `k - 1` elements apart.
- Move both pointers at the same pace until the first pointer reaches the end.
- Return the node that the second pointer is pointing to.
```js
const kthToLast = (head, k) => {
  let pointer1 = head;
  let pointer2 = head;
  
  for (let i = 0; i < k - 1; i++) {
    if (pointer1 === null) return null; // `k` is greater than the length of the list.
    pointer1 = pointer1.next;
  }
  
  while (pointer1.next !== null) {
    pointer1 = pointer1.next;
    pointer2 = pointer2.next;
  }
  
  return pointer2;
}
```

## 2.3 Delete Middle Node
- Input: reference to the middle node of a singly linked list.
- Output: no need to return the linked list but delete the middle node.
### Example
```js
// Before: 1 -> 2 -> 3 -> 4 -> 5
// After: 1 -> 2 -> 4 -> 5
```
### Solution
- Replace the middle node's value with the next node's value.
- Update the middle node's next.
```js
const deleteMiddleNode = (middleNode) => {
  if (middleNode === null || middleNode.next === null) return false;
  
  const nextNode = middleNode.next;
  middleNode.val = nextNode.val;
  middleNode.next = nextNode.next;
  nextNode.next = null;
  return true;
}
```

## 2.4 Partition
- Input: the `head` of a singly linked list, and integer `x`.
- Output: move all nodes with values less than `x` to the "left" of the linked list, and all nodes with values equal to or greater than `x` to the "right" of the linked list.
- Constraints
  - Nodes with a value of `x` do not have to be in between the "left" and "right" partitions. They just have to be somewhere in the "right" partition.
  - The original order does not matter.
### Example
```js
// Inputs:
  // head = 3 -> 5 -> 8 -> 5 -> 10 -> 2 -> 1
  // x = 5
// Output: 3 -> 2 -> 1 -> 5 -> 8 -> 5 -> 10
```
### Solutions
#### Approach 1 - O(n)
- Loop through the linked list and record values in two separate arrays (`lessThanX` and `xOrAbove`).
- Loop through both arrays to replace the value in the current node.
```js
const partition = (head, x) => {
  const lessThanX = [];
  const xOrAbove = [];
  let currNode = head;
  while (currNode !== null) {
    if (currNode.val < x) lessThanX.push(currNode.val);
    else if (currNode.val >= x) xOrAbove.push(currNode.val);
    currNode = currNode.next;
  }
  
  currNode = head;
  for (let num of lessThanX) {
    currNode.val = num;
    currNode = currNode.next;
  }
  for (let num of xOrAbove) {
    currNode.val = num;
    currNode = currNode.next;
  }
  
  return head;
}
```
#### Approach 2 - Rearrange the linked list by updating the head or tail O(n)
- Initialize `newHead` and `tail` to be the given `head`.
- Loop through the linked list.
  - If node's value is less than `x`, update the `newHead` to be that node.
  - If node's value is equal to or greater than `x`, update the `tail` to be that node.
```js
const partition = (head, x) => {
  let newHead = head;
  let tail = head;
  let currNode = head;
  let nextNode;
  
  while (currNode !== null) {
    nextNode = currNode.next;
    if (currNode.val < x) {
      currNode.next = newHead;
      newHead = currNode;
    } else if (currNode.val >= x) {
      tail.next = currNode;
      tail = currNode;
    }
    currNode = nextNode;
  }
  tail.next = null;
  
  return newHead;
}
```

## 2.5 Sum Lists
- Inputs
  - The head of a singly linked list `l1` that represents a number stored in reverse order (one's digit is the head of the linked list).
  - The head of a singly linked list `l2` that represents a number stored in reverse order.
- Output
  - A linked list representing the sum of the two numbers represented by `l1` and `l2`.
### Example
```js
// Inputs
  // l1 = 7 -> 1 -> 6
  // l2 = 5 -> 9 -> 2
// Output: 2 -> 1 -> 9
```
### Solution
- Replicate normal addition process in a recursive way.
- Add the two values and carry over the "extra" digit to the next nodes.
```js
const sumLists = (l1, l2, carry = 0) => {
  if (l1 === null && l2 === null && carry === 0) return null;
  
  const summedList = new Node();
  let value = carry;
  if (l1 !== null) value += l1.val;
  if (l2 !== null) value += l2.val;
  
  summedList.val = value % 10;
  
  if (l1 !== null || l2 !== null) {
    const nextDigit = sumLists(l1.next, l2.next, value = 10 ? 1 : 0);
    summedList.next = nextDigit;
  }
  
  return summedList;
}
```

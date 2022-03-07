# Chapter 2 - Linked Lists

## Return Kth to Last
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

# Singly Linked Lists - Easy

## 237. Delete Node in a Linked List
- Input: the node to be deleted directly.
- Output: updated linked list.
- Constraints
  - No access to the `head` of the list.
  - Node to be deleted is not a tail node.
  - Value of each node in the list is unique.
### Example
```js
// Delete node with value of 5.
4 -> 5 -> 1 -> 9
// Output
4 ------> 1 -> 9

// Solution Logic is more like this.
4 -> 1 ------> 9
```
### Solution
```js
const deleteNode = (node) => {
  // Update node's val to be its next's val.
  node.val = node.next.val;
  // Update node's next to be its next next node.
  node.next = node.next.next;
}
```

## 21. Merge Two Sorted Lists
- Input: the heads of two sorted linked lists `list1` and `list2`.
- Output: return the head of the merged linked list.
### Example
```js
// Input: list1 = [1,2,4], list2 = [1,3,4]
// Output: [1,1,2,3,4,4]
```
### Solution
```js
const mergeTwoLists = (list1, list2) => {
  // Initialize dummy head node to begin linking the merged list (mergedHead.next will be the head of the merged list).
  let mergedHead = { val: -1, next: null };
  // Initialize current node with mergedHead.
  let currNode = mergedHead; // Any manipulation to currNode will be applied to mergedHead.
  
  // While there are still nodes to compare in both lists.
  while (list1 && list2) {
    // If list2's node value is less than list1's node value.
    if (list2.val < list1.val) {
      // Set currNode's next to be list2's node.
      currNode.next = list2;
      // Move list2 node to the next node.
      list2 = list2.next;
    } else {
      // Set currNode's next to be list1's node.
      currNode.next = list1;
      // Move list1 node to the next node.
      list1 = list1.next;
    }
    // Move currNode to the next node.
    currNode = currNode.next;
  }
  
  // If one of the lists has reached the end, point currNode's next to the remaining nodes in the other list.
  currNode.next = list1 || list2;
  
  // Return the head of the merged linked list.
  return mergedHead.next;
}
```
## 203. Remove Linked List Elements
- Input: the `head` of a linked list and an integer `val`.
- Output: return the new `head` of the linked list with all nodes with a value of `val` removed.
### Example
```js
// Input: [6,6,1,6,6,2,3,4,5,6]
// Output: [1,2,3,4,5]

6 -> 6 -> 1 -> 6 -> 6 -> 2 -> 3 -> 4 -> 5 -> 6 -> null
h
          h
          c ------>
          c ----------->
                         c
                              c
                                   c
                                        c
                                        c ------>
```
### Solution
```js
const removeElements = (head, val) => {
  // If there is no head, return it.
  if (!head) return head;
  
  // While head starts out to be val, and immediately following nodes' values are val.
  // Objective: skip ahead of continuous nodes with val.
  while (head && head.val === val) {
    // Move head forwards.
    head = head.next;
  }
  
  // Initialize current node to be head.
  let currNode = head;
  
  // While currNode and currNode's next exist.
  // Objective: skip nodes with values of val.
  while (currNode && currNode.next) {
    // If currNode's next's value is val.
    if (currNode.next.val === val) {
      // Set currNode's next to be its next next.
      currNode.next = currNode.next.next;
    } else {
      // Move currNode forwards.
      currNode = currNode.next;
    }
  }
  
  return head;
}
```

## 83. Remove Duplicates from Sorted List
- Input: the `head` of a sorted linked list.
- Output: return the `head` of the sorted linked list without duplicates.
### Example
```js
// Input: [1,1,2]
// Output: [1,2]

// Input: [1,1,2,3,3]
// Output: [1,2,3]
```
### Solution
```js
const deleteDuplicates = (head) => {
  // Initialize currNode with head.
  let currNode = head;
  
  // While the end of the list hasn't been reached yet.
  while (currNode) {
    // If currNode's next is not null (currNode is not the tail) and currNode's next's value is the same as currNode's value.
    if (currNode.next !== null && currNode.next.val === currNode.val) {
      // Set currNode's next to be its next next.
      currNode.next = currNode.next.next;
    } else {
      // Move currNode forwards.
      currNode = currNode.next;
    }
  }
  
  return head;
}
```

## 160. Intersection of Two Linked Lists
- Input: the heads of two singly linked-lists `headA` and `headB`.
- Output: return the node at which the two lists intersect (`null` if no intersection).
  - *Intersecting node is the first node that is shared by the two lists.*
### Example
```js
// A: 4 -> 1 -> 8 -> 4 -> 5
// B: 5 -> 6 -> 1 -> 8 -> 4 -> 5
// intersectVal = 8.

// Objective: align the two linked lists (line up the ends of the two lists) by concatenating them in opposite orders.

     <---------- A --------->      <------------ B ------------>
A+B: 4 -> 1 -> 8 -> 4 -> 5 -> null -> 5 -> 6 -> 1
                                                  -> 8 -> 4 -> 5
B+A: 5 -> 6 -> 1 -> 8 -> 4 -> 5 -> null -> 4 -> 1
     <------------ B ------------>      <---------- A --------->
```
### Solution
```js
const getIntersectionNode = (headA, headB) => {
  // Initialize copies of headA and headB.
  let a = headA;
  let b = headB;
  
  // While a and b are not referring to the exact same node.
  while (a !== b) {
    // If a is null, concatenate headB. Else, move on to the next node.
    a = a === null ? headB : a.next;
    // If b is null, concatenate headA. Else, move on to the next node.
    b = b === null ? headA : b.next;
  }
  
  // Return either a or b (if no intersection was encountered, it will be null).
  return a;
}
```

## 234. Palindrome Linked List
- Input: the `head` of a singly linked list.
- Output: return `true` if it is a palindrome.
### Example
```js
// Input: head = [1,7,2,8,2,7,1]
// Output: true

// Using Solution 2.
// Find the middle.
1 -> 7 -> 2 -> 8 -> 2 -> 7 -> 1
               s              f
// Reverse the second half.
1 -> 7 -> 2 -> 8 <- 2 <- 7 <- 1
                              f
                                   s
// Compare node values from both ends.
1 -> 7 -> 2 -> 8 <- 2 <- 7 <- 1
f                             s
```
### Solution 1
- TC: O(n)
- SC: O(n)
```js
const isPalindrome = (head) => {
  let str = "";
  while (head) {
    str += head.val;
    head = head.next;
  }
  
  let reversed = "";
  
  for (let i = str.length-1; i >= 0; i--) {
    reversed += str[i];
  }
  
  return str === reversed ? true : false;
}
```
### Solution 2 - Slow and Fast Runner
- TC: O(n)
- SC: O(1)
- **Idea:**
  - Find the middle using slow/fast runner, reverse the second half of the list to the middle.
  - Compare node values starting from both ends to the middle.
```js
const isPalindrome = (head) => {
  // Initialize pointers.
  let slow = head;
  let fast = head;
  
  // While fast hasn't passed/reached the end yet.
  while (fast !== null && fast.next !== null) {
    // Move slow and fast.
    slow = slow.next;
    fast = fast.next.next;
  }
  // At this point, slow is in the middle and fast is at the tail or passed the tail (null).
  
  // Initialize prev and next.
  let prev = slow;
  let next;
  // Move slow.
  slow = slow.next;
  // Set middle node's next to be null to avoid infinite loop.
  prev.next = null;
  // While slow hasn't passed the end yet.
  while (slow !== null) {
    // Preserve next node.
    next = slow.next;
    // Set slow's next to be prev.
    slow.next = prev;
    // Move prev and slow.
    prev = slow;
    slow = next;
  }
  // At this point, slow is the tail's next (null). 
  
  // Set fast to the head, and set slow to the tail (prev).
  fast = head;
  slow = prev;
  
  // While fast and slow have not passed the middle.
  while (slow !== null) {
    // If fast's value and slow's value are not equal, return false.
    if (fast.val !== slow.val) return false;
    else {
      // Move fast and slow to their next nodes.
      fast = fast.next;
      slow = slow.next;
    }
  }
  
  return true;
}
```

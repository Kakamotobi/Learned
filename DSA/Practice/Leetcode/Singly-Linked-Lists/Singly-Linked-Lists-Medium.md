# Singly Linked Lists - Medium

## 1669. Merge In Between Linked Lists
- Input:
  - Two linked lists `list1` and `list2`.
  - `a`th node and `b`th node (0-based).
- Output: return `list1` with its nodes from `a`th to `b`th (incl. `a`th and `b`th) replaced with `list2`.
- Constraints
  - `3 <= list1.length <= 10^4`
  - `1 <= a <= b < list1.length - 1`
  - `1 <= list2.length <= 10^4`
### Example
```js
// Input
List1: 0 -> 1 -> 9 -> 9 -> 9 -> 9 -> 6 -> 7 -> 8
a = 2
b = 7
List2: 2 -> 3 -> 4 -> 5

// Output
List1: 0 -> 1    9 -> 9 -> 9 -> 9    6 -> 7 -> 8
List2:        -> 2 -> 3 -> 4 -> 5 ->
```
### Solution 1
- TC: O(n)
- SC: O(1)
```js
const mergeInBetween = (list1, a, b, list2) => {
  // Initialize currNode.
  let currNode = list1;
  // Initialize ithCounter.
  let ithCounter = 1;
  
  // Objective: move currNode to (a - 1)th node in list1.
  while (ithCounter < a) {
    currNode = currNode.next;
    ithCounter++;
  }
  
  // Preserve reference to ath node with severTarget.
  let severTarget = currNode.next;
  // Link currNode to list2.
  currNode.next = list2;
  
  // Objective: move severTarget to bth node in list1.
  while (ithCounter < b) {
    severTarget = severTarget.next;
    ithCounter++;
  }
  
  // Preserve reference to (b + 1)th node in list1.
  let temp = severTarget.next;
  // Sever bth's node's next.
  severTarget.next = null;
  
  // Objective: move currNode to list2's tail.
  while (currNode.next !== null) {
    currNode = currNode.next;
  }
  
  // Link currNode to temp.
  currNode.next = temp;
  
  return list1;
}
```
### Solution 2
- TC: O(n)
- SC: O(1)
```js
const mergeInBetween = (list1, a, b, list2) => {
  // Initialize targetA and targetB.
  let targetA = list1;
  let targetB = list1;
  
  // Objective: move targetA to (a-1)th node and targetB to (b+1)th node.
  for (let i = 0; i <= b && targetA !== null && targetB !== null) {
    // If i is less than a - 1, move targetA.
    if (i < a-1) targetA = targetA.next;
    // If i is less than or equal to b, move targetB.
    if (i <= b) targetB = targetB.next;
  }
  
  // Link targetA to list2.
  targetA.next = list2;
  
  // Objective: move targetA to list2's tail.
  while (targetA.next !== null) {
    targetA = targetA.next;
  }
  
  // Link list2's tail to targetB.
  targetA.next = targetB;
  
  return list1;
}
```

## 1721. Swapping Nodes in a Linked List
- Input: `head` of a linked list and an integer `k`.
- Output: return the `head` of the linked list after swapping the values of the `k`th node from the beginning and the `k`th node from the end (1-indexed).
### Example
```js
// Input
k = 2
1 -> 2 -> 3 -> 4 -> 5
// Output
1 -> 4 -> 3 -> 2 -> 5

// Input
k = 2
100 -> 90
// Output
90 -> 100
```
### Solution
```js
const swapNodeValues = (head, k) => {
  // Initialize two strands of the list.
  let A = head;
  let B = head;
  
  // Objective: move A to kth node.
  for (let i = 1; i < k; i++) A = A.next;
  // Preserve kthNode.
  let kthNode = A;
  
  // Move A to its next in order to align with B.
  A = A.next;
  // Objective: move B to the kth node from the back.
  while (A !== null) {
    A = A.next;
    B = B.next;
  }
  
  // Swap node values.
  [kthNode.val, B.val] = [B.val, kthNode.val];
  
  return head;
}
```

## 2095. Delete the Middle Node of a Linked List
- Input: the `head` of a singly linked list.
- Output: return the `head` of the list after deleting the middle node.
- Constraints
  - The middle node of a linked list of size `n` is the `[n/2]`th node from the start using 0-based indexing.
### Example
```js
// Input
1 -> 3 -> 4 -> 7 -> 1 -> 2 -> 6
// Output
1 -> 3 -> 4 ------> 1 -> 2 -> 6

// Input
1 -> 2 -> 3 -> 4
// Output
1 -> 2 ------> 4
```
### Solution
```js
const deleteMiddle = (head) => {
  // Initialize prevMid, mid, scout.
  let prevMid = head;
  let mid = head;
  let scout = head;
  // Initialize numNodes.
  let numNodes = 1;
  
  // Objective: get prevMid to be the (middle-1)th node.
  while (scout && scout.next) {
    // Skip the first iteration for prevMid.
    if (numNodes !== 1) prevMid = prevMid.next;
    mid = mid.next;
    scout = scout.next.next;
    numNodes++;
  }
  
  // Edge Case: if length is 1.
  if (numNodes === 1) {
    head = null;
    return head;
  }
  
  // Delete the middle node.
  prevMid.next = mid.next;
  mid.next = null;
  
  return head;
}
```





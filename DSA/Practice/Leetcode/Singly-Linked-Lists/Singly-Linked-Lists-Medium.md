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


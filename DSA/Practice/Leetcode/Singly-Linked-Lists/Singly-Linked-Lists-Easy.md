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

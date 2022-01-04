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

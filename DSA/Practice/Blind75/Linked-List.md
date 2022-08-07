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

## 3. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
### Solution 1
```js
// TC: O(n), SC: O(1)

const mergeTwoLists = (list1, list2) => {
  // Edge Cases: empty list(s)
  if (!list1 && !list2) return list1;
  if (!list1 && list2) return list2;
  if (list1 && !list2) return list1;
  
  let mergedHead;
  let currNode;
  
  // Initialize mergedHead and currNode.
  if (list1.val <= list2.val) {
    mergedHead = list1;
    list1 = list1.next;
  } else {
    mergedHead = list2;
    list2 = list2.next;
  }
  
  currNode = mergedHead;
  
  // Merge lists.
  while (list1 !== null && list2 !== null) {
    if (list1.val <= list2.val) {
      headPointer.next = list1;
      list1 = list1.next;
    } else {
      headPointer.next = list2;
      list2 = list2.next;
    }
    headPointer = headPointer.next;
  }
  
  // If there are any remaining nodes left in either list.
  headPointer.next = list1 || list2;
  
  return mergedHead;
}
```
### Solution 2
- Use a dummy node as the head and add on.
```js
// TC: O(n), SC: O(1)

const mergeTwoLists = (list1, list2) => {
    // Initialize mergedHead as a dummy node.
    let mergedHead = new ListNode(-1, null);
    let currNode = mergedHead;
    
    while (list1 && list2) {
        if (list1.val <= list2.val) {
            currNode.next = list1;
            list1 = list1.next;
        } else {
            currNode.next = list2;
            list2 = list2.next;
        }
        currNode = currNode.next;
    }
    
    currNode.next = list1 || list2;
    
    return mergedHead.next;
}
```

## 4. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
### Solution 1
- Find out the number of nodes in the linked list.
- Place the pointer at the previous node of the target node and surgically remove the target node.
```js
// TC: O(n), SC: O(1)

const removeNthFromEnd = (head, n) => {
  let numNodes = 0;
  let scout = head;
  while (scout !== null) {
    numNodes++;
    scout = scout.next;
  }
  
  // Edge Case: if target node is the first/head node (i.e. n === numNodes).
  if (n === numNodes) {
    let newHead = head.next;
    head.next = null;
    return newHead;
  }
  
  // Get currNode to the node before the target node.
  let currNode = head;
  for (let i = 0; i < numNodes - n - 1; i++) {
    currNode = currNode.next;
  }
  // Remove target node.
  let targetNode = currNode.next;
  currNode.next = targetNode.next;
  targetNode.next = null;
  
  return head;
}
```
### Solution 2 - Two Pointers (ft. Dummy Node)
- Create a dummy node before `head`.
- Use two pointers (offset by `n`) to place the "left" pointer to the node before the target node.
```js
// TC: O(n), SC: O(1)

const removeNthFromEnd = (head, n) => {
  const dummyNode = new ListNode(-1, head);
  
  // Initialize pointers.
  let pointer1 = dummyNode;
  let pointer2 = head;
  // Offset pointer1 and pointer2 by n.
  for (let i = 0; i < n; i++) {
    pointer2 = pointer2.next;
  }
  
  // Get pointer1 to the node before the target node.
  while (pointer2 !== null) {
    pointer1 = pointer1.next;
    pointer2 = pointer2.next;
  }
  
  // Remove target node.
  const targetNode = pointer2.next;
  pointer1.next = targetNode.next;
  targetNode.next = null;
  
  return dummyNode.next;
}
```

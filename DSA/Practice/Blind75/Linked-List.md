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


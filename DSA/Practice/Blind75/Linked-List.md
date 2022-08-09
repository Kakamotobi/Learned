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

## 5. [Reorder List](https://leetcode.com/problems/reorder-list/)
### Solution 1 - Use array
- Accumulate all node values in an array.
- Initialize pointers at each end (excl. index 0).
- Alternate between each pointer by simply using the duality of odd/even numbers.
  - The `right` pointer should go first.
```js
// TC: O(n), SC: O(n)

const reorderList = (head) => {
  const values = [];
  let currNode = head;
  while (currNode !== null) {
    values.push(currNode.val);
    currNode = currNode.next;
  }
  
  currNode = head;
  let left = 1;
  let right = values.length - 1;
  
  for (let i = 0; i < values.length - 1; i++) {
    if (i % 2 !== 0) {
      currNode.next = new ListNode(values[left], null);
      left++;
    } else {
      currNode.next = new ListNode(values[right], null);
      right--;
    }
    currNode = currNode.next;
  }
  
  return head;
}
```
### Solution 2 - Reverse half and merge
- Cut the linked list in half and reverse the back portion.
- Merge the two halves together.
```js
// TC: O(n), SC: O(1)

const reorderList = (head) => {
  // Find the middle node.
  let mid = head;
  let fast = head;
  while (fast && fast.next) {
    mid = mid.next;
    fast = fast.next.next;
  }
  
  // Preserve the reference to the (mid+1)th node and sever the link between the front and back portions.
    // Front Portion: 1 --> 2 --> 3 --> null
    // Back Portion: 4 --> 5
  let backPortion = mid.next;
  mid.next = null;
  
  // Reverse the back portion.
    // Back Portion: 5 --> 4 --> null
  let prevNode = null;
  let nextNode = null;
  while (backPortion !== null) {
    nextNode = backPortion.next;
    backPortion.next = prevNode;
    prevNode = backPortion;
    backPortion = nextNode;
  }
  
  // Merge the front and back portions together.
  let left = head;
  let right = prevNode;
  let leftNext;
  let rightNext;
  while (left && right) {
    // Preserve next nodes.
    leftNext = left.next;
    rightNext = right.next;
    // Reorder.
    left.next = right;
    right.next = leftNext;
    // Move to next nodes.
    left = leftNext;
    right = rightNext;
  }
  
  return head;
}
```

## 6.[Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
### Solution 1 - Sort
- Accumulate all the values into an array.
- Sort the values.
- Create a linked list using those values.
```js
// TC: O(km + n*logn), SC: O(n)
  // k - the number of lists.
  // m - the number of nodes in each list.
  // n - the number of all nodes.

const mergKLists = (lists) => {
  const values = [];
  for (let list of lists) {
    while (list) {
      values.push(list.val);
      list = list.next;
    }
  }
  
  values.sort((a,b) => a - b);
  
  // Checking type handles:
    // first number is 0.
    // empty list and/or lists.
  const head = typeof values[0] === "number" ? new ListNode(values[0]) : null;
  let currNode = head;
  for (let k = 1; k < values.length; k++) {
    currNode.next = new ListNode(values[k]);
    currNode = currNode.next;
  }
  
  return head;
}
```
### Solution 2 - Divide and Conquer (merge two at a time)
- Merge the first two lists of `lists` and push the merged list back to `lists`.
- Repeat this until there is less than one list remaining.
```js
// TC: O(kn * logk), SC: O(1)
  // k - the number of lists.
  // n - the number of nodes in each list.

const mergeTwoLists = (list1, list2) => {
  // Initialize mergedHead to a dummy node.
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

const mergeKLists = function(lists) {
  if (lists.length === 0) return null;

  while (lists.length > 1) {
    lists.push(mergeTwoLists(lists[0], lists[1]));
    lists.shift();
    lists.shift();
  }

  return lists[0];
}
```

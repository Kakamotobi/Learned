# 2022 KAKAO SUMMER TECH INTERNSHIP

## Make Two Queues Sum Equal
- **Inputs**
  - A queue `queue1` represented in an array.
  - A queue `queue2` represented in an array.
- **Description**
  - A single operation consists of popping an element from one queue and pushing it to the other queue.
- **Output:** return the number of operations needed to make the queues have the same sum.
### Example
```js
solution([3, 2, 7, 2], [4, 6, 5, 1]);
solution([1, 2, 1, 2], [1, 10, 1, 2]);
solution([1, 1], [1, 5]);
```
### Solution
- Add the sum of both queues and divide it by 2 to get the target sum.
- Concatenate the two queues into one array.
- Use two pointers to find the point where the target sum is achieved.
  - First pointer initialized to index 0.
  - Second pointer initialized to the start of the second queue.
```js
const solution = (queue1, queue2) => {
  const targetSum = queue1.reduce((sum,x) => sum + x, 0) + queue2.reduce((sum,x) => sum + x, 0);
  const arr = [...queue1, ...queue2];
  let leftPointer = 0;
  let rightPointer = queue1.length;
  while (leftPointer <= rightPointer && rightPointer < arr.length) {
    const runningSum = arr.slice(leftPointer, rightPointer+1).reduce((sum, x) => sum + x, 0);
    if (runningSum < targetSum) rightPointer++;
    else if (runningSum > targetSum) leftPointer++;
    else return leftPointer + (rightPointer - (queue1.length - 1));
  }
  return -1;
}
```

# Heap

## 1. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
### Solution 1 - Hash Map
```js
// TC: O(nlogn), SC: O(n)

const topKFrequent = (nums, k) => {
  const ans = [];
  
  // Collect frequencies of each number.
  const freq = [];
  for (let num of nums) {
    freq[num] = ++freq[num] || 1;
  }
  
  // Order the numbers by frequency.
  const sortedNums = Object.keys(freq).sort((a,b) => freq[b] - freq[a]);
  
  return sortedNums.slice(0, k).map(n => parseInt(n));
}
```
### Solution 2 - Priority Queue
```js
// TC: O(n*logk), SC: O(n)

const topKFrequent = (nums, k) => {
  // Collect frequencies of each number.
  const freq = [];
  for (let num of nums) {
    freq[num] = ++freq[num] || 1;
  }
  
  // Construct priority queue.
  const pq = new PriorityQueue(); // implemented by max binary heap
  for (let num in freq) {
    pq.enqueue(num, freq[num]);
    if (pq.length > k) pq.dequeue(); // keeps it at O(logk) not O(logn)
  }
  
  // Extract `k` elements from pq.
  const ans = [];
  for (let i = 0; i < k; i++) {
    ans.push(pq.dequeue().val);
  }
  return ans;
}
```

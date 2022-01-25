# Binary Heaps - Medium

## 347. Top K Frequent Elements
- Input: integer array `nums` and an integer `k`.
- Output: return the `k` most frequent elements (in any order).
### Example
```js
// Input: nums = [1,1,1,2,2,3], k = 2
// Output: [1,2]
```
### Solution - Priority Queue (Max Binary Heap)
- Use a priority queue where the priority level is each number's frequency in nums.
  - Use max binary heap so that the node with the largest frequency(priority) is at the top.
- Dequeue k times to get the k most frequent elements.
```js
const topKFrequent = (nums, k) => {
  // Collect nums and each frequency.
  const freqMap = {};
  for (let num of nums) {
    freqMap[num] = ++freqMap[num] || 1;
  }
  
  // Construct priority queue (max binary heap).
  const PQ = new PriorityQueue();
  for (let num in freqMap) {
    PQ.enqueue(num, freqMap[num]);
  }
  
  // Extract the k most frequent elements.
  const res = [];
  for (let i = 0; i < k; i++) {
    res.push(PQ.dequeue().val);
  }
    
  return res;
}
```


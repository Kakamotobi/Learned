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

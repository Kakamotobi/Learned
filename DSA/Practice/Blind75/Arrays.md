# Blind 75 - Arrays

## 1. Two Sum
- Create a hash map to store the number along with its index.
- Loop over nums.
  - If other half of current number is in the hash map, return their indices.
  - Store current number and its index in the hash map.
```js
const twoSum = (nums, target) => {
  const record = new Map();
  
  for (let i = 0; i < nums.length; i++) {
    const otherHalf = target - nums[i];
    if (record.has(otherHalf)) {
      return [i, record.get(otherHalf)];
    }
    record.set(nums[i], i);
  }
}
```

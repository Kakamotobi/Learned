# Blind 75 - Arrays

## 1. [Two Sum](https://leetcode.com/problems/two-sum/)
- Create a hash map to store the number along with its index.
- Loop over nums.
  - If other half of current number is in the hash map, return their indices.
  - Store current number and its index in the hash map.
```js
// TC: O(n), SC: O(n)

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

## 2. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- Keep track of maximum profit.
- Keep track of minimum price.
- Loop over prices.
  - Update the minimum price with the current price if necessary.
    - This guarantees lowest buy before selling.
  - Update the maximum profit with `current price - minimum price` if necessary.
    - `current price - minimum price` takes care of no profit since it will result to 0.
### Solution 1
```js
// TC: O(n), SC: O(1)

const maxProfit = (prices) => {
  let maximumProfit = -Infinity;
  let minimumPrice = Infinity;
  
  for (let price of prices) {
    minimumPrice = Math.min(price, minimumPrice);
    maximumProfit = Math.max(price - minimumPrice, maximumProfit); // price - minimumPrice 
  }
  
  return maximumProfit;
}
```
### Solution 2 - Sliding Window
- Use two pointers representing the buy point and sell point.
- While the sell point is not at the end of the prices array,
  - If buy point is greater than sell point, 
    - Move buy point to sell point (this is safe to do because all potential profits in the middle have already been considered).
  - Else if buy point is less than or equal to sell point,
    - Calculate difference and update maximum profit if necessary.
    - Move sell point.
```js
// TC: O(n), SC: O(1)

const maxProfit = (prices) => {
  let maximumProfit = 0;
  
  let buyLowPointer = 0;
  let sellHighPointer = 1;
  
  while (sellHighPointer < prices.length) {
    if (prices[buyLowPointer] > prices[sellHighPointer]) {
      buyLowPointer = sellHighPointer;
    } else if (prices[buyLowPointer <= prices[sellHighPointer]) {
      const profit = prices[sellHighPointer] - prices[buyLowPointer];
      maximumProfit = Math.max(profit, maximumProfit);
      sellHighPointer++;
    }
  }
  
  return maximumProfit;
}
```

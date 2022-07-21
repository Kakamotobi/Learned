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
### Solution 1
- Keep track of maximum profit.
- Keep track of minimum price.
- Loop over prices.
  - Update the minimum price with the current price if necessary.
    - This guarantees lowest buy before selling.
  - Update the maximum profit with `current price - minimum price` if necessary.
    - `current price - minimum price` takes care of no profit since it will result to 0.
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

## 3. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
### Solution 1 - Set
- Convert the array into a set and compare the length.
- If there are duplicates, the length will be different.
```js
// TC: O(n), SC: O(n)

const containsDuplicate = (nums) => {
  const set = new Set(nums);
  
  return set.size !== nums.length;
}
```
### Solution 2 - Set
- Loop over nums.
  - If the set already has the current num, there are duplicates.
  - Add the current num to the set.
```js
const containsDuplicate = (nums) => {
  const set = new Set();
  
  for (let num of nums) {
    if (set.has(n)) return true;
    else set.add(n);
  }
  
  return false;
}
```
### Solution 3 - Sort
- Sort the array in order.
- If any number is the same as the number before it, there are duplicates.
```js
// TC: O(n log n), SC: O(1)

const containsDuplicate = (nums) => {
  nums.sort((a,b) => a - b);
  
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === nums[i-1]) return true;
  }
  
  return false;
}
```

## 4. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- Key Point: a subarray with potentially the largest sum does not start with a negative number (unless all numbers are negative).
```js
// TC: O(n), SC: O(1)

const maxSubArray = (nums) => {
  let largestSum = nums[0];
  let subArraySum = 0;
  
  for (let num of nums) {
    // If the subArraySum up to this point (before num) is negative, reset to 0 (start a new subarray).
    if (subArraySum < 0) subArraySum = 0;
    // Update subArraySum.
    subArraySum += num;
    // Update largest sum.
    largestSum = Math.max(subArraySum, largestSum);
  }
  
  return largestSum;
}
```

## 5. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
### Solution 1 - Brute Force
- For each element in `height`, calculate the area with each of the following elements.
```js
// TC: O(n^2), SC: O(1)

const calcMaxRecArea = (coordA, coordB) => {
  const [x1, y1] = coordA;
  const [x2, y2] = coordB;
  return Math.abs((x1 - x2) * Math.min(y1, y2));
}

const maxArea = (height) => {
  let maximumArea = -Infinity;
  
  for (let i = 0; i < height.length; i++) {
    for (let j = i; j < height.length; j++) {
      maximumArea = Math.max(calcMaxRecArea([i,height[i]], [j,height[j]), maximumArea);
    }
  }
  
  return maximumArea;
}
```
### Solution 2 - Two Pointers
- Initialize two pointers - one at each end.
- Calculate the area, then move the pointer with the less value inwards.
```js
// TC: O(n), SC: O(1)

const calcMaxRecArea = (coordA, coordB) => {
  const [x1, y1] = coordA;
  const [x2, y2] = coordB;
  return Math.abs((x1 - x2) * math.min(y1, y2));
}

const maxArea = (height) => {
  let maximumArea = -Infinity;
  
  let left = 0;
  let right = height.length - 1;
  
  while (left < right) {
    maximumArea = Math.max(calcMaxRecArea([left,height[left]], [right,height[right]]), maximumArea);
    if (height[left] <= height[right]) left++;
    else right--;
  }
  
  return maximumArea;
}
```

## 6. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
### Solution 1 - Dynamic Programming
- Keep track of the current maximum product and current minimum product.
  - The current max and current min represents the max and min product of all the subarrays up to that number (excl. that number).
  - Preserve the current min product to account for negative numbers.
    - If there is an even number of negative numbers, it can end up resulting to the largest product.
```js
// TC: O(n), SC: O(1)

const maxProduct = (nums) => {
  let largestProduct = Math.max(...nums);
  let currMax = 1;
  let currMin = 1;
  let temp;
  
  for (let num of nums) {
    // Update the current max and min product (of all the possible subarrays) to this point, if necessary.
    temp = currMax;
    currMax = Math.max(num * currMax, num * currMin, num);
    currMin = Math.min(num * temp, num * currMin, num);
    // Update the largest product if necessary.
    largestProduct = Math.max(currMax, currMin, largestProduct);
  }
  
  return largestProduct;
}
```
### Solution 2 - Two Pointer Approach (with a twist)
- If all numbers are positive or there is an even number of negative numbers, the largest product will be the product of all the numbers.
- If there is an odd number of negative numbers, **the subarray of the largest product must therefore be a prefix or suffix of the array, since it has to be *contiguous***.
- Handle 0s by simply treating it as a delimiter for separate subarrays.
```js
// TC: O(n), SC: O(1)

const maxProduct = (nums) => {
  let largestProduct = nums[0];
  let runningProductLeft = 1;
  let runningProductRight = 1;
  
  for (let i = 0; i < nums.length; i++) {
    // Handle 0s.
    if (runningProductLeft === 0) runningProductLeft = 1;
    if (runningProductRight === 0) runningProductRight = 1;
    
    // Update the running products of both ends.
    runningProductLeft *= nums[i];
    runningProductRight *= nums[nums.length - 1 - i];
    // Update the alrgest product, if necessary.
    largestProduct = Math.max(runningProductLeft, runningProductRight, largestProduct);
  }
  
  return largestProduct;
}
```

## 7. [3Sum](https://leetcode.com/problems/3sum/)
### Solution 1 - Brute Force
- Find all possible triplets from the array.
- Filter out the triplets that sum to 0.
- Filter out duplicates.
```js
// TC: O(n^3), SC: O(n)

const threeSum = (nums) => {
  const triplets = [];
  
  for (i = 0; i < nums.length; i++) {
    for (let j = i+1; j < nums.length; j++) {
      for (let k = j+1; k < nums.length; k++) {
        if (nums[i] + nums[j] + nums[k] === 0) {
          triplets.push([nums[i], nums[j], nums[k]].sort((a,b) => a - b));
        }
      }
    }
  }
  
  return triplets.filter((temp = {}, (arr) => !(temp[arr] = arr in temp)));
}
```
### Solution 2 - Sort and Pointers
- Sort the array.
  - Now, duplicates are grouped together.
  - This means that the duplicates as the first number of a triplet (in the sorted array) will always end up having the same triplet combination.
    - Ex: `[-3,-3,-1,0,2,3]`.
  - Since we do not want this, we need to skip them.
    - Fortunately, sorting the array makes it easier to skip them.
```js
// TC: O(n^2), SC: O(n)

const threeSum = (nums) => {
  const triplets = [];
  
  nums.sort((a,b) => a - b);
  
  for (let i = 0; i < nums.length; i++) {
    // If i is the same as before, skip it.
    if (i > 0 && nums[i] === nums[i-1]) continue;
    
    let j = i + 1;
    let k = nums.length - 1;
    
    // Find nums[j] and nums[k] so that they add up to sum with nums[i].
    while (j < k) {
      const sum = nums[i] + nums[j] + nums[k];
      
      if (sum < 0) j++;
      else if (sum > 0) k--;
      else if (sum === 0) {
        // Found a valid triplet. 
        triplets.push([nums[i], nums[j], nums[k]]);
        // Check next j.
        j++;
        // If j is the same as before, skip it.
        // This also automatically handles duplicate k.
        while (j < k && nums[j] === nums[j-1]) j++;
      }
    }
  }
  
  return triplets;
}
```

## 8. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self)
### Solution 1 - Brute Force
```js
// TC: O(n^2), SC: O(n)

const productExceptSelf = (nums) => {
  const ans = [];
  
  for (let i = 0; i < nums.length; i++) {
    const arrExclCurrNum = [...nums.slice(0,i), ...nums.slice(i+1)];
    const product = arrExclCurrNum.reduce((prod, num) => prod * num, 1);
    ans.push(product);
  }
  
  return ans;
}
```
### Solution 2 - Prefix and Suffix
- Compute the product of the current number's prefix.
- Compute the product of the current number's suffix.
```js
// TC: O(n), SC: O(n)

const productExceptSelf = (nums) => {
  const ans = [1]; // Initialize with 1 to account for the first number's prefix, which there isn't.
  
  // Prefix products.
  for (let i = 1; i < nums.length; i++) {
    ans.push(nums[i-1] * ans[i-1]);
  }
  
  // Suffix products.
  let suffixProd = 1;
  for (let i = nums.length - 1; i >= 0; i--) {
    ans[i] *= suffixProd;
    // Update suffix product (for the next num).
    suffixProd *= nums[i];
  }
  
  return ans;
}
```

## 9. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
- Tweaked binary search.
- For a rotated sorted array, the numbers of the left subarray are always greater than the numbers on the right subarray.
- Aim is to place the left pointer on the minimum element.
  - If the mid number belongs to the left subarray, update left to be the number after mid.
  - Else if the mid number belongs to the right subarray, update right to be the mid number.
```js
// TC: O(log n), SC: O(1)

const findMin = (nums) => {
  let left = 0;
  let right = nums.length - 1;
  
  while (left < right) {
    const mid = Math.floor((left + right) / 2);
    if (nums[mid] > nums[right]) left = mid + 1; // if nums[mid] belongs to the left subarray.
    else right = mid; // if nums[mid] belongs to the right subarray.
  }
  
  return nums[left];
}
```

## 10. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- Determine if `mid` is in the "left" or "right" sorted subarray (`nums[left] <= nums[mid]` means mid is in "left" sorted subarray).
  - Ex: `[4,5,6,7,0,1,2]`.
- If `mid` is in the "left" sorted subarray, need to handle:
  - `target > nums[mid]` &rarr; search "right" portion.
    - Ex: if `nums[mid]` is `6` and `target` is `7`, search `[7,0,1,2]`.
  - `target < nums[mid]` &rarr; two possibilities:
    - `target >= nums[left]` &rarr; search "left" portion.
      - Ex: if `nums[mid]` is `6` and `target` is `5`, search `[4,5]`.
    - `target < nums[left]` &rarr; search "right" portion.
      - Ex: if `nums[mid]` is `6` and `target` is `1`, search `[7,0,1,2]`.
- Else if `mid` is in the "right" sorted subarray, need to handle:
  - `target < nums[mid]` &rarr; search "left" portion.
    - Ex: if `nums[mid]` is `1` and `target` is `0`, search `[4,5,6,7,0]`.
  - `target > nums[mid]` &rarr; two possibilities:
    - `target > nums[right]` &rarr; search "left" portion.
      - Ex: if `nums[mid]` is `1` and `target` is `6`, search `[4,5,6,7,0]`.
    - `target <= nums[right]` &rarr; search "right" portion.
      - Ex: if `nums[mid]` is `1` and `target` is `2`, search `[2]`.
```js
// TC: O(log n), SC: O(1)

const search = (nums, target) => {
  let left = 0;
  let right = nums.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (nums[mid] === target) return mid;
    
    // mid is in "left" sorted subarray.
    if (nums[left] <= nums[mid]) {
      if (target > nums[mid]) left = mid + 1;
      else if (target < nums[mid]) {
        if (target >= nums[left]) right = mid - 1;
        else if (target < nums[left]) left = mid + 1;
      }
    }
    // mid is in "right" sorted subarray.
    else {
      if (target < nums[mid]) right = mid - 1;
      else if (target > nums[mid]) {
        if (target > nums[right]) right = mid - 1;
        else if (target <= nums[right]) left = mid + 1;
      }
    }
  }
  
  return -1;
}
```

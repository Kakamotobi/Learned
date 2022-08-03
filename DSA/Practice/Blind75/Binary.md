# Binary

## 1. [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
- Use XOR to add the numbers.
- Use AND and left shift to take into account carry overs.
- Repeat this while there are still carry overs.
```js
// TC: O(1), SC: O(1)

const getSum = (a, b) => {
  while (b !== 0) {
    const carryOver = (a & b) << 1;
    a = a ^ b;
    b = carryOver; // after the first iteration, b will represent the carry over.
  }
  return a;
}
```

## 2. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
- Objective: make `n` equal to `0`.
  - If the rightmost bit is a "1", count.
  - Update n by zerofill right shift (drop the least significant bit).
```js
// TC: O(log n), SC: O(1)

const hammingWeight = (n) => {
  let numOnes = 0;
  
  while (n !== 0) {
    numOnes += n & 1;
    n = n >>> 1;
  }
  
  return numOnes;
}
```

## 3. [Reverse Bits](https://leetcode.com/problems/reverse-bits/)
- Determine the rightmost bit of `n` and accumulate.
  - Do this 32 times.
- Note
  - Given input is an unsigned integer.
  - However, the left shift bitwise operator in JS returns a signed 32-bit integer.
  - The signed integer can be converted back to unsigned by using the unsigned right shift (`>>>`).
    - Ex: `num >>> 0` &rarr; sign bit becomes `0`.
```js
// TC: O(1), SC: O(1)

const reverseBits = (n) => {
  let reversed = 0;
  
  for (let i = 0; i < 32; i++) {
    reversed = reversed << 1; // Left shift so that reversed's rightmost bit is 0.
    if (n & 1 === 1) reversed++; // If n's rightmost bit is 1, "make" the reversed's rightmost bit 1 as well.
    n = n >>> 1; // Drop n's rightmost bit.
  }
  
  return reversed >>> 0; // Convert signed to unsigned.
}
```

## 4. [Missing Number](https://leetcode.com/problems/missing-number/)
### Solution 1
- Sort and look for any consecutive `0` or `1` as the LSB.
```js
// TC: O(n log n), SC: O(1)

const missingNumber = (nums) => {
  nums.sort((a,b) => a - b);
  
  // Edge case for if missing number is 0.
  if (nums[0] !== 0) return 0;
  
  for (let i = 0; i < nums.length; i++) {
    if ((nums[i] & 1) === (nums[i-1] & 1)) return i;
  }
  
  // Edge case for if missing number is n.
  return nums.length;
}
```
### Solution 2
- X ^ X is 0, and X ^ 0 is X.
- Therefore, XOR-ing expected nums and given nums will result to the missing number.
- Order does not matter.
#### Implematation 1
```js
// TC: O(n), SC: O(1)

const missingNumber = (nums) => {
  let missingNum = 0;
  
  // XOR given nums.
  for (let num of nums) {
    missingNum = missingNum ^ num;
  }
  
  // XOR expected nums.
  for (let i = 0; i <= nums.length; i++) {
    missingNum = missingNum ^ i;
  }
  
  return missingNum;
}
```
#### Implementation 2
```js
// TC: O(n), SC: O(1)

const missingNumber = (nums) => {
  let missingNum = nums.length;
  
  for (let i = 0; i <= nums.length; i++) {
    missingNum = missingNum ^ nums[i]; // Given nums
    missingNum = missingNum ^ i; // Expected nums
  }
  
  return missingNum;
}
```

## 5. [Counting Bits](https://leetcode.com/problems/counting-bits/)
### Solution 1 - Brute Force
- Same approach as [this](#2-number-of-1-bits).
```js
// TC: O(n log n), SC: O(1)

const getNumOfOnes = (num) => {
  let numOfOnes = 0;
  while (num !== 0) {
    if (num & 1 === 1) numOnes++;
    num = num >>> 1;
  }
  return numOfOnes;
}

const countBits = (n) => {
  const ans = [];
  for (let i = 0; i <= n; i++) {
    ans.push(getNumOfOnes(i));
  }
  return ans;
}
```
### Solution 2 - Dynamic Programming
- When the most significant bit changes, the proceeding bits repeat from the "top" (from `0`).
  - Therefore, offset by MSB's bit place (Ex: 1, 2, 4, 8, 16, ...).
  - This means there's no need to recalculate every bit.
  - Simply, add `1` (MSB) and the value at the offset.
- If number is a power of `2`, update the offset.
```js
const countBits = (n) => {
  const ans = new Array(n+1).fill(0);
  
  let offset = 1;
  
  for (let i = 0; i <= n; i++) {
    // Check if offset needs to be updated.
    if (i === offset * 2) offset = i;
    // Record the number of 1 bits in this number's binary representation.
    // `1` represents the MSB.
    ans[i] = 1 + ans[i - offset];
  }
  
  return ans;
}
```
```js
const countBits = (n) => {
  const ans = new Array(n+1).fill(0);
  
  let pointer = 0;
  
  for (let i = 0; i <= n; i++) {
    if (i === pointer * 2) pointer = 0;
    ans[i] = 1 + ans[pointer];
    pointer++;
  }
  
  return ans;
}
```

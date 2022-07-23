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
const hammingWeight = (n) => {
  let numOnes = 0;
  
  while (n !== 0) {
    numOnes += n & 1;
    n = n >>> 1;
  }
  
  return numOnes;
}
```

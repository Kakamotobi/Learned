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

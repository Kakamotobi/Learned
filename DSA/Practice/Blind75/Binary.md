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

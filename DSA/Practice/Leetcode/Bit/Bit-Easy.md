# Bit - Easy

## Table of Contents
- [231. Power of Two](#231-power-of-two)

## [231. Power of Two](https://leetcode.com/problems/power-of-two/description/)
### Solution 1 - Recursion
```js
// TC: O(log(n)), SC: O(log(n))

const isPowerOfTwo = (n) => {
  if (n === 0) return false;
  if (n === 1) return true;
  if (n % 2 !== 0) return false;
  return isPowerOfTwo(n / 2);
}
```
### Solution 2 - Math
```js
// TC: O(1), SC: O(1)

const isPowerOfTwo = (n) => {
  return Number.isInteger(Math.log2(n));
};
```
### Solution 3 - Bit
- The binary form of a number that is a power of two is always a 1 followed by 0s (Ex: 16: 1000).
- The binary form of a number that is less than a number that is a power of two is always one bit less than that number (Ex: 15: 0111).
- If a number `n` is a power of two, `n & (n-1)` will always be 0.
```js
// TC: O(1), SC: O(1)

const isPowerOfTwo = (n) => {
    return n > 0 && (n & (n - 1)) === 0;
}
```

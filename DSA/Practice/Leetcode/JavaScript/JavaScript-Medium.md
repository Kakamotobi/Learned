# JavaScript Medium

## Table of Contents
- [2622. Cache With Time Limit](#2622-cache-with-time-limit)
- [2624. Snail Traversal](#2624-snail-traversal)
- [2623. Memoize](#2623-memoize)

## [2622. Cache With Time Limit](https://leetcode.com/problems/cache-with-time-limit/)
```js
var TimeLimitedCache = function() {
  this.cache = {};
};

/** 
 * @param {number} key
 * @param {number} value
 * @param {number} time until expiration in ms
 * @return {boolean} if un-expired key already existed
 */
TimeLimitedCache.prototype.set = function(key, value, duration) {
  const keyExists = this.cache[key] ? true : false;

  if (keyExists) clearTimeout(this.cache[key].timeoutId);

  const timeoutId = setTimeout(() => {
    delete this.cache[key];
  }, duration);

  this.cache[key] = { value, timeoutId };

  return keyExists;
};

/** 
 * @param {number} key
 * @return {number} value associated with key
 */
TimeLimitedCache.prototype.get = function(key) {
  return this.cache[key]?.value ?? -1;
};

/** 
 * @return {number} count of non-expired keys
 */
TimeLimitedCache.prototype.count = function() {
  return Object.entries(this.cache).length;
};
```

## [2624. Snail Traversal](https://leetcode.com/problems/snail-traversal/)
```js
/**
 * @param {number} rowsCount
 * @param {number} colsCount
 * @return {Array<Array<number>>}
 */
Array.prototype.snail = function(rowsCount, colsCount) {
  if (rowsCount * colsCount !== this.length) return [];

  let row;
  let col;

  return this.reduce((acc, curr, idx) => {
    row = idx % rowsCount;
    col = Math.floor(idx / rowsCount);

    // Need to reverse on odd columns for "snail" curve.
    if (col % 2 !== 0) {
      row = rowsCount - row - 1;
    }

    if (!acc[row]) acc[row] = [];
    acc[row][col] = curr;

    return acc;
  }, []);
}
```

## [2623. Memoize](https://leetcode.com/problems/memoize/)
```js
/**
 * @param {Function} fn
 */
function memoize(fn) {
    const memo = new Map();
    
    return function(...args) {
        if (memo.get(`${args}`) !== undefined) return memo.get(`${args}`);

        memo.set(`${args}`, fn(...args));
        return memo.get(`${args}`);
    }
}
```

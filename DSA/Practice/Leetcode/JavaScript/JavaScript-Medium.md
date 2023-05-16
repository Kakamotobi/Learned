# JavaScript Medium

## Table of Contents
- [2622. Cache With Time Limit](#2622-cache-with-time-limit)
- [2624. Snail Traversal](#2624-snail-traversal)
- [2623. Memoize](#2623-memoize)
- [2625. Flatten Deeply Nested Array](#2625-flatten-deeply-nested-array)
- [2633. Convert Object to JSON String](#2633-convert-object-to-json-string)

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

## [2625. Flatten Deeply Nested Array](https://leetcode.com/problems/flatten-deeply-nested-array/)
```js
/**
 * @param {any[]} arr
 * @param {number} depth
 * @return {any[]}
 */
const flat = (arr, n) => {
    if (n === 0) return arr;

    const res = [];

    arr.forEach((el) => {
        // if el is an array, need recursion check (for possible nested arrays).
        if (Array.isArray(el)) res.push(...flat(el, n-1));
        // else just push to res.
        else res.push(el);
    })

    return res;
};
```

## [2633. Convert Object to JSON String](https://leetcode.com/problems/convert-object-to-json-string/)
```js
/**
 * @param {any} object
 * @return {string}
 */
const jsonStringify = function(object) {
    const helper = (val) => {
        switch (typeof val) {
            case "object":
                if (Array.isArray(val)) {
                    return `[${val.map((x) => helper(x)).join(",")}]`;
                } else if (val) {
                    return `{${Object.keys(val).map((key) => `"${key}":${helper(val[key])}`).join(",")}}`;
                } else {
                    return "null";
                }
            case "string":
                return `"${val}"`;
            case "number":
                return val;
            case "boolean":
                return val;
            default:
                return "";
        }
    }

    return helper(object);
};
```

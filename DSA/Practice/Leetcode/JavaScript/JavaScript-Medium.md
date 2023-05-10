# JavaScript Medium

## Table of Contents
- [2622. Cache With Time Limit](#2622-cache-with-time-limit)

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

/**
 * Your TimeLimitedCache object will be instantiated and called as such:
 * var obj = new TimeLimitedCache()
 * obj.set(1, 42, 1000); // false
 * obj.get(1) // 42
 * obj.count() // 1
 */
```

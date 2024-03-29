# JavaScript Medium

## Table of Contents
- [2622. Cache With Time Limit](#2622-cache-with-time-limit)
- [2624. Snail Traversal](#2624-snail-traversal)
- [2623. Memoize](#2623-memoize)
- [2625. Flatten Deeply Nested Array](#2625-flatten-deeply-nested-array)
- [2633. Convert Object to JSON String](#2633-convert-object-to-json-string)
- [2632. Curry](#2632-curry)
- [2676. Throttle](#2676-throttle)
- [2694. Event Emitter](#2694-event-emitter)
- [2700. Differences Between Two Objects](#2700-differences-between-two-objects)
- [2721. Execute Asynchronous Functions in Parallel](#2721-execute-asynchronous-functions-in-parallel)

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
  });

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

## [2632. Curry](https://leetcode.com/problems/curry/)
```js
/**
 * @param {Function} fn
 * @return {Function}
 */
const curry = function(fn) {
  return function curried(...args) {
    // If equal or more arguments than the number of parameters that `fn` expects was given, execute and return the output.
    if (args.length >= fn.length) {
      return fn(...args);
    }

    return function(...moreArgs) {
      return curried(...args, ...moreArgs);
    }
  };
};
```

## [2676. Throttle](https://leetcode.com/problems/throttle/description/)
- Countdown for `t` starts upon the first function call.
- The arguments of any function calls in that period of time will be stored, where the arguments of the latest function call will overwrite that of its preceding function call.
- When the countdown ends, the function should be called again with latest saved arguments (repeat cycle).

```js
/**
 * @param {Function} fn
 * @param {number} t
 * @return {Function}
 */
const throttle = function(fn, t) {
  let latestArgs;
  let throttlingActive = false;

  function executeFn() {
    // If there is no timer in place (i.e. can execute the function right away).
    if (!throttlingActive && latestArgs) {
      fn (...latestArgs);

      latestArgs = null;
      throttlingActive = true;

      setTimeout(() => {
        throttlingActive = false;
        executeFn();
      }, t);
    }
  }

  return function(...args) {
    latestArgs = args;
    executeFn();
  }
};
```

## [2694. Event Emitter](https://leetcode.com/problems/event-emitter/description/)
```js
class EventEmitter {
  constructor() {
    this.subscriptions = {};
  }

  subscribe(event, cb) {
    if (this.subscriptions[event]) this.subscriptions[event].push(cb);
    else this.subscriptions[event] = [cb];

    return {
      unsubscribe: () => {
        this.subscriptions[event] = this.subscriptions[event].filter((subscriber) => {
          return subscriber !== cb;
        });
      }
    };
  }

  emit(event, args = []) {
    if (!this.subscriptions[event]) return [];

    return this.subscriptions[event].reduce((acc, curr) => {
      return [...acc, curr(...args)];
    }, []);
  }
}
```

## [2700. Differences Between Two Objects](https://leetcode.com/problems/differences-between-two-objects/description/)
```js
function objDiff(obj1, obj2) {
  function helper(obj1Item, obj2Item) {
    // "Same" Case
    // If the same, return an empty array.
    if (obj1Item === obj2Item) return [];

    // "Different" Cases
    // If either is null.
    // If either is not an object.
    // If one is an array and the other is not.
    if ((obj1Item === null || obj2Item === null) ||
      (!isObject(obj1Item) || !isObject(obj2Item)) ||
      (Array.isArray(obj1Item) !== Array.isArray(obj2Item))
    ) {
      return [obj1Item, obj2Item];
    }

    // At this point, both are not null || both are objects || both are arrays
    // i.e. both are iterable (need to check inside).
    console.log(obj1Item, obj2Item);

    const diffs = {};

    for (let obj1Key in obj1Item) {
      if (obj1Key in obj2Item) {
        const innerDiffs = helper(obj1Item[obj1Key], obj2Item[obj1Key]);

        if (Object.keys(innerDiffs).length > 0) {
          diffs[obj1Key] = innerDiffs;
        }
      }
    }

    return diffs;
  }

  function isObject(target) {
    return typeof target === "object";
  }

  return helper(obj1, obj2);
};
```

## [2721. Execute Asynchronous Functions in Parallel](https://leetcode.com/problems/execute-asynchronous-functions-in-parallel/)
```js
const promiseAll = async function(functions) {
  return new Promise(async (resolve, reject) => {
    const res = [];
    let numResolvedFn = 0;
    
    // Higher order functions do not wait for promises to resolve before moving to the next iteration.
    functions.forEach(async (fn, idx) => {
      try {
        res[idx] = await fn();
        numResolvedFn++;
        
        if (numResolvedFn === functions.length) {
          resolve(res);
        }
        
        // Doesn't work when using `idx` because this will execute before all promises (`fn()`) have resolved.
        // if (idx === functions.length - 1) {
          //     resolve(res);
        // }
      } catch(err) {
        reject(err);
      }
    });
  });
};
```

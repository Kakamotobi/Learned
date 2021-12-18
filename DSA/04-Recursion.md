# Recursion

## Table of Contents
- [What is Recursion?](#what-is-recursion)
  - [Recursion and the Call Stack](#recursion-and-the-call-stack)
- [Pure Recursion Examples](#pure-recursion-examples)
  - [Pure Recursion Tips](#pure-recursion-tips)
- [Helper Method Recursion](#helper-method-recursion)

## What is Recursion?
- **A process (a function in this case) that calls itself.**
- **Invoke the same function over and over again with a different input each time until you reach your base case.**
- The two essential parts of a recursive function.
  - **Different Input:** the function is called with a different input each time.
  - **Base Case:** the condition when the recursion ends.
- Examples
  - `JSON.parse()`, `JSON.stringify()` are often written recursively (based on the browser).
  - `document.getElementById` and DOM traversal algorithms.
  - Object traversal.
  - Used in traversing more complex data structures as well.
- c.f. Iteration.
  - Recursion is sometimes a cleaner alternative to iteration.
### Recursion and the Call Stack
- Call Stack
  - A stack data structure that manages function calls.
  - Any time a function is invoked, it is placed on top of the call stack (FILO/LIFO).
  - When JS encounters the `return` keyword, or when the function ends, the compiler will remove that function from the call stack.
- ***When we write recursive functions, we keep pushing the same new functions onto the call stack.***

## Pure Recursion Examples
```js
function countDown(num) {
  // Base Case
  if (num <= 0) {
    console.log("All done!");
    return;
  }
  
  console.log(num);
  num--;
  countDown(num);
}
```
```js
function sumRange(num) {
  // Base Case
  if (num === 1) return 1;
  
  return num + sumRange(num - 1);
}

// sumRange(3)
   // return 3 + sumRange(3-1)
                 // return 2 + sumRange(2-1)
                               // return 1
```
```js
function factorialRecursion(num) {
  // Base Case
  if (num === 1) return 1;
  
  return num * factorial(num - 1);
}

// factorial(3)
   // return 3 * factorial(3 - 1)
                 // return 2 * factorial(2 - 1)
                               // 1
```
### Pure Recursion Tips
- For arrays, use methods like `slice`, the spread operator, and `concat` that make copies of arrays so you do not mutate them.
  - Allows you to accumulate some sort of result.
- Strings are immutable to use methods like `slice`, `substr`, or `substring` to make copies of strings.
- To make copies of objects, use `Object.assign` or the spread operator.

## Helper Method Recursion
- A design pattern commonly used with recursion.
- **A recursive helper function inside of a non-recursive outer function.**
- Commonly used when you need to compile something like an array or a list of data.
### Pattern Flow
```js
function outer(input) {
  let outerScopedVariable = [];
  
  function helper(helperInput) {
    // modify the outerScopedVariable
    helper(helperInput--);
  }
  
  helper(input);
  
  return outerScopedVariable;
}
```
### Example
```js
// Helper Method Recursion

function collectOddValues(arr) {
  let result = [];
  
  function helper(helperInput) {
    // If empty, return.
    if (helperInput.length === 0) {
      return;
    }
    // If the first element is odd, push it to the result array.
    if (helperInput[0] % 2 !== 0) {
      result.push(helperInput[0];
    }
    // Call helper function again, excluding the first element.
    helper(helperInput.slice(1));
  }
  
  helper(arr);
  
  return result;
}
```
```js
// Pure Recursion Approach

function collectOddValues(arr) {
  let newArr = [];
  
  // If arr is empty, return newArr.
  if (arr.length === 0) {
    return newArr;
  }
  // If the first element is odd, push it to newArr.
  if (arr[0] % 2 !== 0) {
    newArr.push(arr[0]);
  }
  
  // Call 
  newArr = newArr.concat(collectOddValues(arr.slice(1)));
  return newArr;
}

// collectOddValues([1,2,3])
   // [1].concat(collectOddValues([2,3]))
                 // [].concat(collectOddValues([3]))
                              // [3].concat(collectOddValues([]))
                                            // return []
```

## 

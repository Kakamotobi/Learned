# Recursion

## Table of Contents
- [What is Recursion?](#what-is-recursion)
  - [Recursion and the Call Stack](#recursion-and-the-call-stack)
- [Pure Recursion Examples](#pure-recursion-examples)
  - [Pure Recursion Tips](#pure-recursion-tips)
- [Helper Method Recursion](#helper-method-recursion)
- [Practice Problems](#practice-problems)

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

## Practice Problems
### Problem 1
- Write a recursive function called reverse, which accepts a string and returns a new string in reverse.
#### Expectation
```js
reverse("awesome") // "emosewa"
reverse("rithmschool") // "loohcsmhtir"
```
#### Helper Method Recursion Solution
```js
function reverse(str) {
  let reversedStr = "";
  
  function helper(helperInput) {
    // Base Case
    if (helperInput === "") return;
    
    // Add the last char of str onto reversedStr.
    reversedStr += helperInput[helperInput.length-1];
    
    // Call (excluding last char of str).
    return helper(helperInput.slice(0, helperInput.length-1);
  }
  
  helper(str);
  
  return reversedStr;
}
```
#### Pure Recursion Solution 1
```js
function reverse(str) {
  let reversedStr = "";
  
  // Base Case
  if (str === "") return "";
  
  // Add the last char of str onto reversedStr.
  reversedStr += str[str.length-1];

  // Call (excluding last char of str).
  reversedStr += reverse(str.slice(0, str.length-1));
  
  // return what has been accumulated in reversedStr (essentially, individual chars).
  return reversedStr;
}

// reverse("hello")
// "o" + reverse("hell")
         // "l" + reverse("hel")
                  // "l" + reverse("he")
                           // "e" + reverse("h")
                                    // "h" + reverse("")
                                             // ""
// "olleh"
```
#### Pure Recursion Solution 2
```js
function reverse(str) {
  // Base Case
  if (str.length <= 1) return str;
  
  // Call (exclude the first char and add it to the end).
  return reverse(str.slice(1)) + str[0];
}

// reverse("hello")
// reverse("ello") + "h"
// reverse("llo") + "e" + "h"
// reverse("lo") + "l" + "e" + "h"
// reverse("o") + "l" + "l" + "e" + "h"
// "o" + "l" + "l" + "e" + "h"
// "olleh"
```
### Problem 2
- Write a recursive function called isPalindrome, which returns true if the string passed to it is a palindrome (reads the same forward and backward). Otherwise, it returns false.
#### Expectation
```js
isPalindrome("awesome") // false
isPalindrome("tacocat") // true
isPalindrome("abcdefgfedcba") // true
```
#### Helper Method Recursion Solution
```js
function isPalindrome(str) {
  let reversedStr = "";
  
  function helper(helperInput) {
    // Base Case
    if (helperInput === "") return;
    
    reversedStr += helperInput[helperInput.length-1];
    
    // Call (excluding last char of str)
    return helper(helperInput.slice(0, helperInput.length-1));
  }
  
  helper(str);
  
  if (str === reversedStr) return true;
  else return false;
}
```
#### Pure Recursion Solution
```js
function isPalindrome(str) {
  if (str.length === 1) return true;
  if (str.length === 2) return str[0] === str[1];
  if(str[0] === str.slice(-1)) return isPalindrome(str.slice(1,-1));
  return false;
}

// isPalindrome("tacocat")
// "t" === "t" so return isPalindrome("acoca")
// "a" === "a" so return isPalindrome("coc")
// "c" === "c" so return isPalindrome("o")
// str.length === 1 so return true

// isPalindrome("abcddcba")
// "a" === "a" so return isPalindrome("bcddcb")
// "b" === "b" so return isPalindrome("cddc")
// "c" === "c" so return isPalindrome("dd")
// str.length === 2 so return "d" === "d"
// true
```
### Problem 3
- Write a recursive function called someRecursive, which accepts an array and a callback. The function returns true if a single value in the array returns true when passed to the callback. Otherwise it returns false.
#### Expectation
```js
const isOdd = val => val % 2 !== 0;
someRecursive([1,2,3,4], isOdd) // true
someRecursive([4,6,8], isOdd) // false

someRecursive([4,6,8], val => val > 10) // false
```
#### Pure Recursion Solution
```js
function someRecursive(arr, callback) {
  // Base Case
  if (arr.length === 0) return false;
  
  if (callback(arr[0]) === true) return true;
  
  // Call (arr excluding first element)
  return someRecursive(arr.slice(1), callback);
}

// someRecursive([8,6,3], isOdd)
// isOdd(8) is false. So, someRecursive([6,3], isOdd)
// isOdd(6) is false. So, someRecursive([3], isOdd)
// isOdd(3) is true. So true.
```
### Problem 4
- Write a recursive function called flatten, which accepts an array of arrays and returns a new array with all values flattened.
#### Expectation
```js
flatten([1,2,3,[4,5]]) // [1,2,3,4,5]
flatten([[1],[2],[3]]) // [1,2,3]
flatten([1,[2,[3,4],[[5]]]]) // [1,2,3,4,5]
```
#### Pure Recursion Solution
```js
function flatten(arr) {
  let flattenedArr = [];
  
  // Base Case
  if (arr.length === 0) return [];
  
  // Deal with first element of arr
  if (Array.isArray(arr[0])) {
    flattenedArr = flatten(arr[0]);
  } else {
    flattenedArr.push(arr[0]);
  }
  
  // Call (excluding the first element of arr)
  return flattenedArr.concat(flatten(arr.slice(1)));
}

// flatten([1,2,3,[4,5]])
// [1].concat(flatten([2,3,[4,5]]))
              // [2].concat(flatten([3,[4,5]]))
                            // [3].concat(flatten([[4,5]]))
                                          // flatten([4,5])
                                             // [4].concat(flatten([5]))
                                                    // [5].concat(flatten([]))
                                                                  // []
// [1,2,3,4,5]
```
### Problem 5
- Write a recursive function called capitalizeFirst. Given an array of strings, capitalize the first letter of each string in the array.
#### Expectation
```js
capitalizeFirst(["car", "taco", "banana"]) // ["Car, "Taco", "Banana"]
```
#### Helper Method Recursion Solution
```js
function capitalizeFirst(arr) {
	let newArr = [];

	function helper(helperInput) {
		// Base Case
		if (helperInput.length === 0) return;

		// Capitalize first letter
		helperInput[0] = helperInput[0].charAt(0).toUpperCase() + helperInput[0].slice(1);
		newArr.push(helperInput[0]);

		// Call
		helper(helperInput.slice(1));
	}

	helper(arr);

	return newArr;
}
```
### Problem 6
- Input: positive integer `n`.
- Output: return the `n`th number in the Fibonnaci sequence.
- Fibonnaci Sequence
	- Every number thereafter is equal to the sum of the previous two numbers.
#### Expectation
```js
// Fibonnaci Sequence: 1, 1, 2, 3, 5, 8, 13, 21, 44 ...
// f(n) = f(n-1) + f(n-2)

fib(5)
fib(4)+fib(3)
fib(3)+fib(2) + fib(2)+fib(1)
fib(2)+fib(1)+fib(1)+fib(0) + fib(1)+fib(0)+fib(1)
fib(1)+fib(0)+fib(1)+fib(1)+fib(0) + fib(1)+fib(0)+fib(1)
1+0+1+1+0 + 1+0+1
5
```
#### Pure Recursion Solution
```js
function fib(n) {
	// Base Case
	if (n === 0) return 0;
	if (n === 1) return 1;
	
	// Recursive Call
	return fib(n - 1) + fib(n - 2);
}
```

### Problem 7
- Write a recursive function called nestedEvenSum, which returns the sum of all even numbers in an object which may contain nested objects.
#### Expectation
```js
const obj1 = {
  outer: 2,
  obj: {
    inner: 2,
    otherObj: {
      superInner: 2,
      notANumber: true,
      alsoNotANumber: "yup"
    }
  }
};

nestedEvenSum(obj1) // 6
```
#### Pure Recursion Solution
```js
function nestedEvenSum (obj) {
	let sum = 0;

	// Essentially Base Case
	if (typeof obj === "object") {
		for (let key in obj) {
			// If even number, add to sum.
			if (obj[key] % 2 === 0) sum += obj[key];
			// If is an object, recursive call.
			if (typeof obj[key] === "object") sum += nestedEvenSum(obj[key]);
		}
	}

	return sum;
}

// nestedEvenSum(obj1)
// 2 + nestedEvenSum(obj)
	   // 2 + nestedEvenSum(otherObj)
	   		  // 2
```
### Problem 8
- Write a recursive function called capitalizeWord, which accepts an array of words and returns an array containing each word capitalized.
#### Expectation
```js
capitalizedWords(["i", "am", "learning", "recursion"]) // ["I", "AM", "LEARNING", "RECURSION"]
```
#### Pure Recursion Solution
```js
function capitalizeWords(words) {
	let capitalizedWords = [];
	// Base Case
	if (words.length === 0) return [];

	capitalizedWords.push(words[0].toUpperCase());

	// Call
	return capitalizedWords.concat(capitalizeWords(words.slice(1)));
}

// capitalizedWords(['i', 'am', 'learning', 'recursion'])
// ["I"].concat(capitalizeWords(["am", "learning", "recursion"]))
								// ["AM"].concat(capitalizeWords(["learning", "recursion"]))
								 								 // ["LEARNING"].concat(capitalizeWords(["recursion"]))
								 																				// ["RECURSION"].concat(capitalizeWords([]))
								 																																// []
```
### Problem 9
- Write a function called stringifyNumbers, which takes in an object and finds all of the values which are numbers and converts them to strings.
#### Expectation
```js
let obj = {
	num: 1,
	test: [],
	data: {
		val: 4,
		info: {
			isRight: true,
			random: 66
		}
	}
}

stringifyNumbers(obj)
// obj = {
//   num: "1",
//   test: [],
//   data: {
//     val: "4",
//     info: {
//       isRight: true,
//       random: "66"
//     }
//   }
// }
```
#### Pure Recursion Solution
```js
function stringifyNumbers(obj) {
	const newObj = {};

	for (let key in obj) {
		if (typeof obj[key] === "number") {
			newObj[key] = obj[key].toString();
		} else if (typeof obj[key] === "object" && !Array.isArray(obj[key])) {
			newObj[key] = stringifyNumbers(obj[key]);
		} else {
			newObj[key] = obj[key];
		}
	}
	
	return newObj;
}

// let obj = {num: 1, test: [], data: {val: 4, info: {isRight: true, random: 66}}}
// stringifyNumbers(obj)
// newObj = {num: "1", test: [], data: stringifyNumbers(obj[key])}
// newObj = {num: "1", test: [], data: {val: "4", info: stringifyNumbers(obj[key])}}
// newObj = {num: "1", test: [], data: {val: "4", info: {isRight: true, random: "66"}}}
```
### Problem 10
- Write a function called collectStrings which accepts an object and returns an array of all the values in the object that have a type of string.
#### Expectation
```js
const obj = {
	stuff: "foo",
	data: {
		val: {
			thing: {
				info: "bar",
				moreInfo: {
					evenMoreInfo: {
						weMadeIt: "baz"
					}
				}
			}
		}
	}
}

collectStrings(obj) // ["foo", "bar", "baz"]
```
#### Helper Method Recursion Solution
```js
function collectStrings(obj) {
	let arr = [];

	function helper(helperInput) {
		for (let key in helperInput) {
			if (typeof helperInput[key] === "string") {
				arr.push(helperInput[key]);
			}
			if (typeof helperInput[key] === "object") {
				return helper(helperInput[key]);
			}
		}
	}

	helper(obj);

	return arr;
}

// collectStrings(obj)
// ["foo"]
// collectStrings(data)
// collectStrings(collectStrings(val))
// collectStrings(collectStrings(collectStrings(thing)))
// ["foo", "bar"]
// collectStrings(collectStrings(collectStrings(collectStrings(moreInfo))))
// collectStrings(collectStrings(collectStrings(collectStrings(collectStrings(evenMoreInfo)))))
// ["foo", "bar", "baz"]
```
#### Pure Recursion Solution
```js
function collectStrings(obj) {
	let arr = [];
	
	for (let key in obj) {
		if (typeof obj[key] === "string") {
			arr.push(obj[key]);
		}
		if (typeof obj[key] === "object") {
			return arr = arr.concat(collectStrings(obj[key]));
		}
	}
	
	return arr;
}

// collectStrings(obj)
// ["foo"].concat(collectStrings(obj[data]))
                  // collectStrings(obj[val])
                     // collectStrings(obj[thing])
                        // ["bar"].concat(collectStrings(obj[moreInfo]))
                                          // collectStrings(obj[evenMoreInfo])
                                             // ["baz"]
```

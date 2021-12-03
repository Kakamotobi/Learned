# Basics of Testing

## Table of Contents
- [Goals of Testing](#goals-of-testing)
- [How to Test](#how-to-test)
- [Simple Example - Node.js](#simple-example---nodejs)
  - [Issues with the Example](#issues-with-the-example)
  - [Solution](#solution)
- [Mocha.js](#mochajs)

## Goals of Testing
- Automate the process of ensuring that our app works as we expect.
- Keep us from having to manually click around the app to find bugs.
- Make sure the app still works as expected even after we change something.
### Opinion
- There are many testing frameworks/libraries out there that people spend more time setting it up than they do writing tests.
- All testing frameworks/libraries are essentially the same.
- We don't even have to use a testing library.

## How to Test
- We will run one testing command that will start up a sub program that is going to make sure that all of our code works as expected.
- No need to know the internal implementation of the code to test. We can test it by knowing what goes into it and what should come out.

## Simple Example - Node.js
```js
// index.js

module.exports = {
	forEach(arr, fn) {
		for (let index in arr) {
			fn(arr[index], index);
		}
	},
	map(arr, fn) {
		const result = [];
		for (let i = 0; i < arr.length; i++) {
			result.push(fn(arr[i], i));
		}
		return result;
	},
};
```
```js
// index.test.js

const { forEach, map } = require("./index.js");

// forEach test
let sum = 0;
forEach([1, 2, 3], (value) => {
	sum += value;
});

if (sum !== 6) {
	throw new Error("Expected summing array to equal 6");
};

// map test
const result = map([1, 2, 3], (value) => {
  return value * 2;
});

if (result[0] !== 2) {
  throw new Error(`Expected to find 2, but found ${result[0]}`);
}
if (result[1] !== 4) {
  throw new Error(`Expected to find 4, but found ${result[1]}`);
}
if (result[2] !== 6) {
  throw new Error(`Expected to find 6, but found ${result[2]}`);
}
```
```zsh
node index.test.js
```
- If no error has occured, the test was successful.
### Issues with the Example
1) Variables are defined in the global scope. So there cannot be another "result" variable for another test.
2) An Error will stop the execution of the test. Hence, proceeding tests will not run.
3) If an error occurs, it is hard to find where that error occured.
### Solution
```js
// index.test.js

const { forEach, map } = require("./index.js");

// Test Helper Function
const test = (desc, fn) => {
	console.log("---", desc);
	try {
		fn();
	} catch (err) {
		console.log(err.message);
	}
};

// forEach Test
test("forEach function", () => {
	let sum = 0;
	forEach([1, 2, 3], (value) => {
		sum += value;
	});

	if (sum !== 6) {
		throw new Error("Expected summing array to equal 6");
	}
});

// map Test
test("map function", () => {
	const result = map([1, 2, 3], (value) => {
		return value * 2;
	});

	if (result[0] !== 2) {
		throw new Error(`Expected to find 2, but found ${result[0]}`);
	}
	if (result[1] !== 4) {
		throw new Error(`Expected to find 4, but found ${result[1]}`);
	}
	if (result[2] !== 6) {
		throw new Error(`Expected to find 6, but found ${result[2]}`);
	}
});
```
### `assert.strictEqual()`
- Node.js has a built-in assertion testing that we can use instead of multiple `if` statements.
- Syntax: `assert.strictEqual(actual, expected[, message])`
```js
// index.test.js

const assert = require("assert");
const { forEach, map } = require("./index.js");

// Test Helper Function
const test = (desc, fn) => {
	console.log("---", desc);
	try {
		fn();
	} catch (err) {
		console.log(err.message);
	}
};

// forEach Test
test("forEach function", () => {
	let sum = 0;
	forEach([1, 2, 3], (value) => {
		sum += value;
	});

	assert.strictEqual(sum, 6, "Expected forEach to sum the array");
});

// map Test
test("map function", () => {
	const result = map([1, 2, 3], (value) => {
		return value * 2;
	});

	assert.strictEqual(result[0], 2);
	assert.strictEqual(result[1], 4);
	assert.strictEqual(result[2], 6);
  
  // OR
	// assert.deepStrictEqual(result, [2, 4, 6]);
});
```

## Mocha.js
- A JavaScript test framework running on Node.js and in the browser.
- A command line tool that we can use to execute all the different tests inside our project.
- Ex: `mocha index.test.js`
- But first, adjust test file to match mocha.
  - No need of the test helper function that we created.
  - Use `it()` instead.
```js
// index.test.js

const assert = require("assert");
const { forEach, map } = require("./index.js");

// forEach Test
it("forEach function", () => {
	let sum = 0;
	forEach([1, 2, 3], (value) => {
		sum += value;
	});

	assert.strictEqual(sum, 6, "Expected forEach to sum the array");
});

// map Test
it("map function", () => {
	const result = map([1, 2, 3], (value) => {
		return value * 2;
	});

	assert.deepStrictEqual(result, [2, 4, 6]);
});
```
```zsh
mocha index.test.js
```

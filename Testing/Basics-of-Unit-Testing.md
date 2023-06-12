# Basics of Unit Testing

## Table of Contents
- [What is Unit Testing?](#what-is-unit-testing)
- [Goals of Testing](#goals-of-testing)
- [Some Concepts](#some-concepts)
	- [Mocking](#mocking)
	- [Interface](#interface)
- [How to Test](#how-to-test)
- [Simple Example - Node.js](#simple-example---nodejs)
  - [Issues with the Example](#issues-with-the-example)
  - [Solution](#solution)
- [Mocha.js](#mochajs)
- [Testing in the Browser](#testing-in-the-browser)
	- [Mocha in the Browser](#mocha-in-the-browser)
	- [Chai.js](#chaijs)

## What is Unit Testing?
> Unit testing is a software development process in which the smallest testable parts of an application, called units, are individually and independently scrutinized for proper operation. ... The main objective of unit testing is to isolate written code to test and determine if it works as intended. | TechTarget

- A program is usually a **layered architecture** where each separate layer combine together to form a single unit of software.
	- A layer should only interact with a layer that is directly above or below it.
	- For example, hardware (I/O) --> OS --> Process --> Thread --> V8.
- Unit testing involves _**testing each layer separately**_.
	- _Validating each layer means that the whole unit would work as expected._
- Unit Testing is a component of **Test-Driven Development (TDD)**.
	- TDD refers to continal testing and revision while building a project.
- It is the first level of testing.
- _Unit tests only test data and functionality, and will not catch errors in integration._

## Goals of Testing
- Automate the process of ensuring that our app works as we expect.
- Keep us from having to manually click around the app to find bugs.
- Make sure the app still works as expected even after we change something.
- Help detect flaws in code earlier on.
### Opinion
- There are many testing frameworks/libraries out there that people spend more time setting it up than they do writing tests.
- All testing frameworks/libraries are essentially the same.
- We don't even have to use a testing library.

## Some Concepts
### Mocking
- Mocking is used when the unit that is being tested has external dependencies such as fetching data from an API.
	- When testing the unit, the result of the external dependency is not what we are testing for. Therefore, we can simply 
### Interface
- When testing, we need to consider as much cases that arise from interfaces including functions, parameters, etc.

## How to Test
- Run one testing command that will start up a sub program that is going to make sure that all of our code works as expected.
- No need to know the internal implementation of the code to test. We can test it by knowing what goes into it and what should come out.
### Notes
- Test each test case independently.
- Do not make a test for every line of code. Make tests for code that affects the behavior of the program.
	- Only test things that are vital to the performance of the unit being tested.
- Once all units in a program have been tested to success, larger components of the program can be tested through integration testing.

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

## Testing in the Browser
- Testing Node.js code is easy and straightforward but testing code in the browser is more challenging and involves more setup because of the DOM.
### Mocha in the Browser
- Setup an HTML document that is dedicated specifically to testing different things inside of our application.
- Ex: Make sure test.html loads up autocomplete widget. And inside of autocomplete.test.js, write code that tests the autocomplete widget.
- If we try to reuse the same widget between tests, the test will break. So after every test, delete the widget that is being displayed on the screen and recreate it from scratch for the next test.
	- **`beforeEach()`**
		- Hook built-in to Mocha.
		- We can write code that is going to setup our testing environment for every single test.
### Chai.js
- The official recommendation is to make use of the Chai.js assertion/expectation library along with Mocha.js.
- It is a library that allows us to use the same kind of assert statements available with Node.
	- Gives you 3 different ways of doing the exact same thing:
		- `chai.should()`
		- `chai.expect()`
		- `chai.assert()`

## Reference
[What is Unit Testing? - TechTarget](https://www.techtarget.com/searchsoftwarequality/definition/unit-testing#:~:text=Unit%20testing%20is%20a%20software,developers%20and%20sometimes%20QA%20staff.)  
[Mocha.js](https://mochajs.org/)  
[Chai.js](https://www.chaijs.com/)  

# Scope
- The location where a variable is defined dictates where we have access to that variable.
	- i.e. a scope indicates where a variable is valid to use, and also what its meaning is.
- Names are valid only until the scope finishes its execution, and "free variables" are looked in the outer scope(s).

## Table of Contents
- [Function Scope](#function-scope)
- [Block Scope](#block-scope)
- [Lexical Scope (a.k.a. Static Scope)](#lexical-scope-aka-static-scope)
	- [Closure](#closure)
		- [How/Where is it Used?](#howwhere-is-it-used)
		- [Closure in JS](#closure-in-js)
			- [Closure Examples](#closure-examples)
			- [Non Closure Examples](#non-closure-examples)
			- [Closure and `for` loops](#closure-and-for-loops)
- [Module Scope](#module-scope)

## Function Scope
- Variables that are defined inside of a function are scoped to that function. So, they cannot be accessed outside of the function.

## Block Scope
- Conditionals, loops, etc.
- Curly braces `{}`, *excluding functions*, are blocks.
### Side Note
- `let` and `const` are scoped to both functions and blocks.
- `var` is scoped to functions but not blocks.

## Lexical Scope (a.k.a. Static Scope)
- "Lexical" refers to something in relation to creating words, expressions, and variables.
- **An item's Lexical Scope is the scope in which it was created (not where it was invoked).**
	- Example
		```js
		const hello = "hello"; // hello's lexical scope is the global scope.
		
		function sayHello() { // sayHello's lexical scope is the global scope.
			return hello;
		}
		```
- **To access an item, the *accessor* has to be within the item's Lexical Scope.**
	- An inner function nested inside of a parent function has access to the variables defined in the scope of the parent function.
  	- The number of nestings doesn't matter.
	- However, a parent function does not have access to its descendant's variables.
	- Example
		```js
		function outer() {
			const name = "Tom";
			function inner() {
				//inner() is the inner function, a closure.
				alert(name);
			}
			inner();
		}
		outer();
		```
- **The inner scope's meaning has precedence over its lexical scope's.**
	- JavaScript looks for variables in (1) its local execution context, then (2) its calling context, then (3) the global execution context.
	- If it doesn’t find the variable, `undefined` is returned.
### Closure
> A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment within which that function was declared). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time. | MDN

> In programming languages, a **closure**, also **lexical closure** or **function closure**, is a technique for implementing lexically scoped name binding in a language with first-class functions. Operationally, a closure is a record storing a function together with an environment. The environment is a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or reference to which the name was bound when the closure was created. Unlike a plain function, a closure allows the function to access those captured variables through the closure's copies of their values or references, even when the function is invoked outside their scope. | Wikipedia

- **Closure is the idea of storing the "free variables" along with the function, so that those variables can be resolved when the function is invoked.**
	- The range of "free variables" depends on the language.
	- Therefore, deeply nested functions could deter performance and use a lot of memory.
- [**Relationship with Lambda**](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Lambda-Calculus.md#relationship-with-closure)
#### How/Where is it Used?
- When a new function is declared and assigned to a variable, the function definition is stored, *as well as a **closure***.
  - ***The closure contains all the variables that are in scope at the time of creation of the function.***
- Closures are frequently used with callbacks (Ex: for event handlers in JS).
#### Closure in JS
- **In JS, the variables captured in a closure includes those in the function itself, those in its lexical environment, and global variables (although technically not enclosed in the closure).**
  - i.e. the whole scope chain.
##### Closure Examples
###### Example 1
- For `test`, which is a reference to `outer()`, its instance of `inner()` still holds a reference to its lexical environment (the `name` variable) - closure. Therefore, `test` still works as expected.
- Henceforth changes to the closure will remain.
	```js
	const name = "Jerry";

	function outer() {
		const name = "Tom";

		function inner() {
			alert(name);
		}

		return inner; // `inner` is a closure (it will still have a reference to the `name` variable from its lexical environment).
	}

	const test = outer(); // The function `inner` is returned with the variable `name` set to `"Tom"`.
	test(); // Calling this returned function will alert with `"Tom"` and not `"Jerry"` because of closures.
	```
###### Example 2
- Despite being deeply nested, `funcD` has access to the variable `n` that is three levels up its scope. It also has access the global variables.
	```js
	function funcA() {
		// closure created!
		let n = 0;
		return function funcB() {
			return function funcC() {
				return function funcD() {
					// closure created
					n++;
					return n;
				};
			};
		};
	}

	// `test` is essentially a bundle of the function definition of `funcD` and its closure.
	const test = funcA()()();
	test(); // 1
	test(); // 2
	```
	- cf. other languages may limit the closure to provide access to only the immediate enclosing scope. This means that they cannot access variables that are further up the scope chain.
###### Example 3
```js
let x = 1;

function f(a) {
  return a + x; // x = 1
}

function g() {
  let x = 2;
  return f(x); // x = 2 because the closer scope has precedence
}

g(); // 3
```
##### Non Closure Examples
###### Example 1
- Global variables are not local variables that are enclosed in a function. Therefore, `funcA` is not a closure.
  ```js
  let n = 5;

  function funcA(x, y) {
    return x + y + n;
  }

  funcA(2, 3); // 10
  ```
###### Example 2
- A closure is a function that has access to variables in its outer scope, _even after the outer function has returned_.
- Once `outer` is returned, `inner` does not have access to variables in `outer`. Therefore, `inner` is not a closure.
  ```js
  function outer() {
    const name = "Tom";

    inner(); // `inner()` is merely a function that is declared and called in `outer()`.

    function inner() {
      alert(name);
    }
  }

  outer();
  ```
##### Closure and `for` loops
- A function in a `for` loop that uses `var` looks for `i` in the scope above it first, which is the function `outer`.
- The functions are stacked in the call stack and when the `for` loop reaches the end, they are executed one by one from the top.
	- At this point, `i` is `3`. Therefore, `3` is printed for all calls.
		- To avoid this, wrap the `setTimeout` in an IIFE OR use block scope (`let`, `const`).
```js
function outer() {
	for (var i = 0; i < 3; i++) { // Note: using `const` or `let` applies block scope.
		setTimeout(() => console.log(i), 0);
	}
}

outer();
// 3
// 3
// 3
```

## Module Scope
- Variables that are defined inside of a module are scoped to that module.
- Unlike function scopes, module scopes can make their variables accessible by other modules as well (`import`, `export`).

## Reference
[Closures - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)  
[Closure (computer programming) - Wikipedia](<https://en.wikipedia.org/wiki/Closure_(computer_programming)>)  
[Master the JavaScript Interview: What is a Closure? | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)  
[I never understood JavaScript closures](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)  
[Is lexical scope the same definition as closure in JavaScript? If not, what are the differences in meaning and example? - Quora](https://www.quora.com/Is-lexical-scope-the-same-definition-as-closure-in-JavaScript-If-not-what-are-the-differences-in-meaning-and-example)  
[WONISM | 자바스크립트 클로저](https://wonism.github.io/closure/)  

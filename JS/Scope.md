# Scope
- The location where a variable is defined dictates where we have access to that variable.
	- i.e. a scope indicates where a variable is valid to use, and also what its meaning is.
- Names are valid only until the scope finishes its execution, and "free variables" are looked in the outer scope(s).

## Table of Contents
- [Function Scope](#function-scope)
- [Block Scope](#block-scope)
- [Lexical Scope (a.k.a. Static Scope)](#lexical-scope-aka-static-scope)
	- [Closure](#closure)
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

- **Closure is the idea of storing the "free variables" along with the function, so that those variables can be resolved when the function is invoked.**
- Functions in JavaScript form closures.
  - When a new function is declared and assigned to a variable, the function definition is stored, *as well as a **closure**.*
  - ***The closure contains all the variables that are in scope at the time of creation of the function.***
- **Closures control what is and isn't in scope in a particular function.**
- **You can use a closure by exposing it.**
	- For example, return the inner function from the parent function or pass it to another function.
	- The inner function will still access the outer function's scope, even after the outer function has returned.
	- Example
		```js
		function outer() {
			const name = "Tom";

			function inner() {
				//inner() is the inner function, a closure.
				alert(name);
			}

			return inner;
		}

		const test = outer();
		test(); // alerts "Tom" because the variable `name` is included in the closure.
		```
#### Example
```js
const name = "Jerry";

function outer() {
	const name = "Tom";
	
	function inner() {
		alert(name);
	}
	
	return inner;
}

const myFunc = outer(); // The function `inner` is returned with the variable `name` set to `"Tom"`.
myFunc(); // Calling this returned function will alert with `"Tom"` and not `"Jerry"` because of closures.
```
- For `myFunc`, which is a reference to `outer()`, its instance of `inner()` still holds a reference to its lexical environment (the `name` variable) - closure. Therefore, `myFunc` still works as expected.
- Henceforth changes to the closure will remain.

## Module Scope
- Variables that are defined inside of a module are scoped to that module.
- Unlike function scopes, module scopes can make their variables accessible by other modules as well (`import`, `export`).

## Reference
[Closures - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)  
[Master the JavaScript Interview: What is a Closure? | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)  
[I never understood JavaScript closures](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)  
[Is lexical scope the same definition as closure in JavaScript? If not, what are the differences in meaning and example? - Quora](https://www.quora.com/Is-lexical-scope-the-same-definition-as-closure-in-JavaScript-If-not-what-are-the-differences-in-meaning-and-example)  

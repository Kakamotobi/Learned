# Scope
- The location where a variable is defined dictates where we have access to that variable.

## Function Scope
- Variables that are defined inside of a function are scoped to that function. So, it can't be accessed outside of the function.

## Block Scope
- Conditionals, loops, etc.
- Curly braces `{}`, *excluding functions*, are blocks.
### Side Note
- `let` and `const` are scoped to both functions and blocks.
- `var` is scoped to functions but not blocks.

## Lexical Scope
- **An inner function nested inside of a parent function has access to the variables defined in the scope of the parent function.**
  - The number of nestings doesn't matter.
- **However, a parent function does not have access to its descendant's variables.**
- Example
	```js
	function outer() {
		const name = "Tom";
		function inner() {
			//inner() is the inner funciton, a closure.
			alert(name);
		}
		inner();
	}
	outer();
	```
- Nested functions have access to variables that are declared in their outer scope. But the opposite is not allowed.
- ***JavaScript looks for variables in its local execution context first, then its calling context, then the global execution context.*** If it doesn’t find the variable, `undefined` is returned.
### Closure
> A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment within which that function was declared). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time. | MDN
- Functions in JavaScript form closures.
  - When a new function is declared and assigned to a variable, the function definition is stored, *as well as a closure.*
  - *The closure contains all the variables that are in scope at the time of creation of the function.*
- Example
	```js
	function outer() {
		const name = "Tom";
		function inner() {
			alert(name);
		}
		return inner;
	}

	const myFunc = outer();
	myFunc();
	```
  - For `myFunc`, which is a reference to `outer()`, its instance of `inner()` still holds a reference to its lexical environment (the `name` variable) - closure. Therefore, `myFunc` still works as expected.
  - Henceforth changes to the closure will remain.

## Reference
[Closures - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)  
[I never understood JavaScript closures](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)  

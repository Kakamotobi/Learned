# Hoisting

Variable and function **declarations** (not initializations) are all given space in memory before they get an actual value, but stay exactly where you typed them in your code.  
They are not physically hoisted to the top of the script.


## Variable Hoisting

### Explanation

#### Case 1 - Reference Error
```
console.log(dog)
```

#### Case 2 - No Error but Variable is Undefined - Hoisting
```
console.log(dog);
var dog = "Buck";
```
- JS is essentially aware of the variable that has been defined later on and gives the value of undefined at the beginning (given space in memory) before execution.
- By hoisting, it is the same as below:
```
var dog; // variable has been created without any value (undefined).
console.log(dog); // undefined variable is printed.
dog = "Buck"; // now, the variable is given a value.
```

#### Case 3 - Successful
```
var dog = "Buck";
console.log(dog);
```

### Example
```
var DEFAULT_RATE = 0.1;
var rate = 0.05;

function getRate() {
  // essentially, variable rate is hoisted here before the code runs.
  // var rate;
  if (!rate) {
    // rate = DEFAULT_RATE due to hoisting.
    var rate = DEFAULT_RATE;
  }
  
  return rate;
}

console.log("Your rate is: " getRate());
```
- Expect to get 0.05. However, returns 0.1.
- `var rate = DEFAULT_RATE` is not scoped to the `if{}` only. It is scoped to the entire function `getRate()`.
  - So, before anything runs, the variable 'rate' is declared.
  - So, rate is undefined, which is falsy. Hence, the `if{}` condition runs.
- Variables defined in functions have precedence over variables defined in the global scope.

### Work-Around
- Use `let` and `const`.
  - Both are also hoisted but are not set to undefined.
    - Therefore, leads to reference error if the variable is used before the initialization occurs.
  - Both are scoped to blocks (ex: conditionals, loops, etc.) rather than current execution contexts (ex: functions) like `var` does.

## Function Hoisting
- Allows you to use a function before it being declared in your code.  
- Function expressions are not hoisted.

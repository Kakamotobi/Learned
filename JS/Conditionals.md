# Conditionals

## if...else Statements
- Executes a statment if a specified condition is truthy.
- If the condition is falsy, the next statement can be executed.
- Syntax:
  ```
  if (condition1) {
    statement1
  } else if (condition2) {
    statement2
  } else if (condition3) {
    statmement3
  } else {
    statementN
  }
  ```

## Nesting Conditionals
- If both/all the conditions are met, execute.
- Ex:
  ```
  if (password.length >= 6) {
    if (password.indexOf(" ") === -1) {
      console.log("Valid Password");
    } else {
      console.log("Cannot contain spaces");
    }
  } else {
    console.log("Must contain at least 6 characters");
  }
  ```
  
## Switch
- Another control-flow statement that can replace multiple `if` statements.
- Syntax:
  ```
  switch(expression) {
    case value1:
      // statements that execute when the result of expression matches value1.
      break;
    case value2:
      // statements that execute when the result of expression matches value2.
      break;
    case value3:
      // statements that execute when the result of expression matches value3.
      break;
    case valueN:
      // statements that execute when the result of expression matches valueN.
      break;
    default:
      // statements that execute when the result of expression doesn't match any of the values.
  }
  ```

## Conditional(Ternary) Operator
- Shortcut for the `if` statement.
- Syntax: `condition ? expressionIfTrue : expressionIfFalse`
- Ex: `(age >= 19) ? "adult" : "minor"; `
- Chained Ternary Operator (alternative to the `if...else` chain)
    ```
    function example(...) {
      return condition1 ? value1
        : condition2 ? value2
        : condition3 ? value3
        : valueN;
    ```

# Scope
- The location where a variable is defined dictates where we have access to that variable.

## Function Scope
- Variables that are defined inside of a function are scoped to that function. So, it can't be accessed outside of the function.

## Block Scope
- Conditionals, loops, etc.
- Curly braces {}, *excluding functions*, are blocks.

### Side Note
- `let` and `const` are scoped to both functions and blocks.
- `var` is scoped to functions but not blocks.

## Lexical Scope
- An inner function nested inside of a parent function has access to the variables defined in the scope of the parent function.
- The number of nestings doesn't matter.
- However, a parent function does not have access to its descendant's variables.


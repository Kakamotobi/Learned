# FP vs OOP

## Table of Contents
- [Value vs Identifier](#value-vs-identifier)
  - [Value](#value)
  - [Identifier](#identifier)

## Value vs Identifier
### Value
- FP is based on values.
- Concerned with only the value. Unconcerned with the memory address.
- Stable against changes in state since the state is not _mutated_.
  - i.e. updates to state means a new value is given instead of mutating the original.
- Logic is based on calculation.
  - Therefore, writing logic for complex domains is difficult.
### Identifier
- OOP is based on identifiers.
  - The only occasion to accept a value in OOP is the `constructor`.
- Concerned with the memory address. Unconcerned with the value.
- Handles changes in state internally.
  - i.e. the state's value is updated through mutation, which could be prone to inconsistencies.
- Logic is based on messages.
  - Therefore, logic can be delegated.

## Reference
[객체지향의 기본 이론 | 개발자 황준일](https://junilhwang.github.io/TIL/CodeSpitz/Object-Oriented-Javascript/01-Intro/#value-vs-identifier)  

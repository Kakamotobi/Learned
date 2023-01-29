# Automata Theory

## Table of Contents
- [What is Automata Theory?](#what-is-automata-theory)
  - [What is an Automaton?](#what-is-an-automaton)
  - [Why use Automata?](#why-use-automata)
  - [Applications of Automata Theory in Web Development](#applications-of-automata-theory-in-web-development)
  - [The Four Families of Automaton](#the-four-families-of-automaton)
- [Formal Grammar](#formal-grammar)
  - [Chomsky's Hierarchy](#chomskys-hierarchy)
  - [Regular Expression](#regular-expression)
    - [Regular Expression and Automata](#regular-expression-and-automata)

## What is Automata Theory?
> Automata theory is a theoretical branch of computer science. It studies abstract mathematical machines called automatons. When given a finite set of inputs, these automatons automatically imitate humans performing tasks by going through a finite sequence of states. | Educative

> ...automata theory deals with the logic of computation with respect to simple machines, referred to as automata. Through automata, computer scientists are able to understand how machines compute functions and solve problems and more importantly, what it means for a function to be defined as computable or for a question to be described as decidable. | Stanford

- Basically, Automata Theory is concerned with how to create systems that can process, manipulate and respond to different types of input in a predictable way.
- Automata does not "remember" state. Therefore, it cannot solve too complicated problems.
  - i.e. something of infinite state cannot be processed using regular expression.
- A finite set of possible states.
  - A state transitions into another state based on a given rule.
- The compiler theory is essentially based on the Automata Theory.

<p align="center">
  <img src="https://prod-discovery.edx-cdn.org/media/course/image/f0e739b2-40cc-49bf-8f24-11a41b54ac16-2355c3022040.small.png" alt="Automata Theory" width="80%"/><br/>
  <em>standford.edu</em>
</p>

- Each node represents a state.
- Each arrow represents a state transition based on a particular rule.

### What is an Automaton?
- An automaton is an abstract machine that performs computations on an input by following a fixed set of instructions.
- Key Assumptions
  - A machine must have a finite description.
  - The input is expressed as a finite string of symbols that come from a fixed alphabet.
### Why use Automata?
- Automatons are easier to build and require no hardware, since they are not physical machines. However, it has its limits (Ex: only capable of computing problems of finite nature).
### Applications of Automata Theory in Web Development
- Web Scraping
  - Extracting and parsing structured data from web pages.
- Text Processing
  - Processing and analyzing text data.
- Web Security
  - Detecting and preventing security attacks on web applications.
- Web Search
- Web Automation
  - Automating repetitive tasks (Ex: filling out forms, naviating through pages, clicking buttons).
### The Four Families of Automaton
- Finite-State Machine
- Pushdown Automata
- Linear-bounded Automata
- Turing Machine

## Formal Grammar
- A **formal grammar** determines how valid strings are formed from a set of alphabet. It has nothing to do with the meaning of a string. It only determines the string's form/syntax.
  - i.e. a finite set of rules, or a schema.
### Chomsky's Hierarchy
- According to Noam Chomsky, there are four types of (formal) grammar.

| **Type** | **Language**           | **Automaton**                                   |
| -------- | ---------------------- | ----------------------------------------------- |
| Type-0   | Recursively enumerable | Turing Machine                                  |
| Type-1   | Context-sensitive      | Linear-bounded non-deterministic Turing machine |
| Type-2   | Context-free           | Non-deterministic pushdown automaton            |
| Type-3   | Regular                | Finite state automaton                          |

### Regular Expression
- Regular Expression refers to a sequence of characters defining a search pattern.
  - i.e. pattern matching.
- It is a combination of characters (letters and numbers) and special/reserved characters in the context of regular expression.
- It is often used in validating user input such as emails.
#### Regular Expression and Automata
- A programming language is defined based on some regular grammar, and can be expressed per those rules.
- Regular Expression is an application of the Automata Theory.
  - As a finite automaton, regular expression is limited to finite states. Hence, it cannot manage infinite state (Ex: keeping track of opening/closing tags).

## Reference
[What is automata theory? - Educative](https://www.educative.io/answers/what-is-automata-theory)  
[Basics of Automata Theory - Stanford](https://cs.stanford.edu/people/eroberts/courses/soco/projects/2004-05/automata-theory/basics.html)  
[Introduction to Automata Theory](https://cs.lmu.edu/~ray/notes/automatatheory/)  
[Chomsky hierarchy - Wikipedia](https://en.wikipedia.org/wiki/Chomsky_hierarchy)  

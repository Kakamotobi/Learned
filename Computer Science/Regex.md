# Regular Expression (Regex)

## Table of Contents
- [What is Regular Expression?](#what-is-regular-expression)
- [Flags](#flags)
- [Metacharacters](#metacharacters)
- [Assertions](#assertions)
  - [Positive Look-Behind](#positive-look-behind)
  - [Negative Look-Behind](#negative-look-behind)
  - [Positive Look-Ahead](#positive-look-ahead)
  - [Negative Look-Ahead](#negative-look-ahead)
- [Non-Capturing Group (`(?:)`)](#non-capturing-group-)
- [Regular Expression in JavaScript](#regular-expression-in-javascript)
- [Examples](#examples)
- [Reference](#reference)

## What is Regular Expression?
> Regular expressions are patterns used to match character combinations in strings. In JavaScript, regular expressions are also objects. These patterns are used with the `exec()` and `test()` methods of `RegExp`, and with the `match()`, `matchAll()`, `replace()`, `replaceAll()`, `search()`, and `split()` methods of `String`. | MDN
- Use Cases
  - Validate user generated input.
  - Search through text (ex: find and replace operation).

## Flags
| Flag    | Description                                                                                   |
| ------- | --------------------------------------------------------------------------------------------- |
| **`d`** | Generate indices for substring matches.                                                       |
| **`g`** | Global search.                                                                                |
| **`i`** | Case-insensitive search.                                                                      |
| **`m`** | Multi-line search.                                                                            |
| **`s`** | Allows `.` to match newline characters.                                                       |
| **`u`** | "unicode"; treat a pattern as a sequence of unicode code points.                              |
| **`y`** | Perform a "sticky" search that matches starting at the current position in the target string. |

## Metacharacters
| Metacharacters | Description   |
| -------------- | ------------- |
| **`+`**        | <ul><li>Match 1 or more of the preceding token.</li><li>i.e., match as many as possible in a row.</li><li>Ex: `/e+/g` will match `e`, `ee`, `eeeeeeeeee`.</li></ul> |
| **`?`**        | <ul><li>Match between 0 and 1 of the preceding token.</li><li>i.e., preceding token is optional.</li><li>Ex: `/e?/g` will match `e`s.</li></ul> |
| **`*`**        | <ul><li>Match 0 or more of the preceding token.</li><li>Basically a combination of `+` and `?`.</li><li>i.e., the preceding token is optional but can match as many as possible in a row.</li><li>Ex: `/re*/g` will match `r`, `re`, `reeeeee`.</li></ul> |
| **`.`**        | <ul><li>Match *any character* except line breaks.</li><li>Ex: `/.at/g` will match `eat`, `cat`, `.at`, ` at`.</li><li>Ex: `/at./g` will match `atm`, `ate`, `at.`, `at `.</li></ul> |
| **`\|`**       | <ul><li>Match the expression before or after the `\|`.</li><li>Ex: `/t\|T/` will match `t`, `T`.</li></ul> |
| **`\`**        | <ul><li>Escape special characters and constructs.</li><li>Ex: `/\./g` will match `.`s.</li><li>Ex: `/\d/g` will match any digit.</li><li>Ex: `/\w/g` will match any word character (letters).</li><li>Ex: `/\s/g` will match any form of whitespace.</li><li>*Capitalize the constructs to get the negative version.*<ul><li>Ex: `/\D/g` will match anything that is not a digit.</li></ul></li></ul>|
| **`{}`**       | <ul><li>Used to specify the length of the match.</li><li>Ex: `/\w{2}/g` will match exactly words with two characters.</li><li>Ex: `/\w{2,}/g` will match words with two or more characters.</li><li>Ex: `/\w{2,5}/g` will match words between two and five characters (inclusive).</li></ul> |
| **`[]`**       | <ul><li>Match any character in the set.</li><li>Ex: `/[cf]at/g` will match `cat` and `fat`.</li><li>Ex: `/[a-zA-Z]at/g` will match any three letter word starting with lowercase and uppercase alphabets a to z and ending with `at`.</li><li>Ex: `/[0-9]/g` will match digits between 0 and 9.</li></ul> |
| **`()`**       | <ul><li>Group multiple tokens together and create a capture group for extracting a substring or using a backreference.</li><li>Ex: `/(T\|t)he/g` will match `The` and `the`.</li><li>Brackets are important because `/(T\|t)he/g` and `/T\|the/g` are very different.<ul><li>`/T\|the/g` will match `T` and `the`.</li></ul></li><li>Ex: `/(T\|t){2}he/g` will match `TThe`, `Tthe`, `tThe`, `tthe`.</li><li>Ex: `/(re){2,3}/g` will match `rere`, `rerere`.</li><li>It is possible to give names to groups.<ul><li>Ex: `/(?<name>\w)/g`</li></ul></li><li>It also stores the part of the string that was matched by the part of the regex inside the parentheses.<ul><li>Ex: `"tomato".split(/(ma)/)` returns `["to", "ma", "to"]`.</li><li>cf. `"tomato".split("ma")` returns `["to", "to"]`.</li></ul></li></ul> |
| **`^`**        | <ul><li>Match the beginning of the string, or the beginning of a line if the multiline flag (`m`) is enabled.</li><li>Ex: `/^T/g` will match `T` only if it is at the beginning of the whole text.</li><li>Ex: `/^T/gm` will match `T`s at the beginning of each line.</li></ul> |
| **`$`**        | <ul><li>Match the end of the string, or the end of the line if the multiline flag (`m`) is enabled.</li><li>Ex: `/\.$/g` will match `.` only if it is at the end of the whole text.</li><li>Ex: `/\.$/gm` will match `.`s at the end of each line.</li></ul> |

## Assertions
> Assertions include boundaries, which indicate the beginnings and endings of lines and words, and other patterns indicating in some way that a match is possible. | MDN
### Positive Look-Behind
- Match anything preceded by the specified expression.
- Syntax: `/(?<=)/g`
  - `<` means look behind.
  - `=` means it is positive but must match what is inside of the group.
- Ex: `/(?<=[Tt]he)./g` will match any character preceded by `[Tt]he` such as the space in `The `, and `n` in `then`, etc.
### Negative Look-Behind
- Match anything that is not preceded by the specified expression.
- Syntax: `/(?<!)/g`
  - `!` means negative.
- Ex: `/(?<![Tt]he)./g` will match any character not preceded by `[Tt]he` such as the `n` in `then`.
### Positive Look-Ahead
- Match a group after the main expression without including it in the result.
- Syntax: `/(?=)/g`
- Ex: `/.(?=at)/g` will match any character that is proceeded by `at` such as the `c` in `cat`, and the `e` in `caveat`.
- Ex: `/\w+(?=at)/g` will match the word that ends in `at` such as `cave` in `caveat`.
### Negative Look-Ahead
- Match anything that is not proceeded by the specified expression.
- Syntax: `/(?!)/g`
- Ex: `/.(?!at)/g` will match any character that is not proceeded by `at`.

## Non-Capturing Group (`(?:)`)
- When you want to match a particular pattern but only to use it to match something else.
  - i.e. you don't want that particular pattern to appear in the ultimate list of matches.
- Ex: `/(?<=\d)(?=(?:\d{3})+$)/g`
  - The `(?:)` in `(?:\d{3})` means match anything and a group of three digits but don't include the group of three digits.
- Ex: `([0-9]+)(?:st|nd|rd|th)?`
  - Match all numbers that are in the form of `1`, `2`, `3`, or `1st`, `2nd`, `3rd` but only ultimately match the numbers (don't include the characters `st`, `nd`, etc).

## Regular Expression in JavaScript
### Two Ways to Construct a RegEx
#### Regular Expression Literal
- The regular expression is compiled when the script is loaded. Therefore, using regular expression literals can improve performance if the regex stays constant.
```js
const re = /ab+c/flags;
```
#### `RegExp` Object
- The regular expression is compiled at runtime. Therefore, use the `RegExp` Object if the regex will change, or will be getting it from another source (ex: user input).
```js
const re = new RegExp("ab+c", "flags");
```
### JavaScript Methods for Regular Expressions
[Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#using_regular_expressions_in_javascript)

## Examples
### Example 1
- Capture phone number.
```
/(?:(\+82)[ -])?\(?(?<areacode>\d{3})\)?[ -]?(\d{4})[ -]?(\d{4})/g

01012345678
010-1234-5678
010 1234 5678
(010) 1234-5678
+82 010 1234 5678
```

## Reference
[Regular expressions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)  
[Learn Regular Expressions in 20 Minutes - YouTube](https://www.youtube.com/watch?v=rhzKDrUiJVk&ab_channel=WebDevSimplified)  
[regex - What is a non-capturing group in regular expressions? - Stack Overflow](https://stackoverflow.com/questions/3512471/what-is-a-non-capturing-group-in-regular-expressions)  
[RegExr: Learn, Build, & Test RegEx](https://regexr.com/)  

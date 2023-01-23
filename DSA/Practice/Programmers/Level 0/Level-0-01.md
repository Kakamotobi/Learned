# Level 0 - 01

## [옹알이 (1)](https://school.programmers.co.kr/learn/courses/30/lessons/120956)
### Solution
- Remove all occurrences of `"aya"`, `"ye"`, `"woo"`, `"ma"` in all strings in `babbling`.
- Strings that end up empty (`""`) are pronouncable words.
```js
const solution = (babbling) => {
  const parsedWords = babbling.map(word => word.replace(/aya|ye|woo|ma/g, ""));
  const filteredWords = parsedWords.filter(word => word === "");
  return filteredWords.length;
}
```

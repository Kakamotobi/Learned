# Backtracking Medium

## Table of Contents
- [Letter Combinations of a Phone Number](#letter-combinations-of-a-phone-number)

## [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
```js
const letterCombinations = (digits) => {
  const res = [];

  const map = {
    2: ["a", "b", "c"],
    3: ["d", "e", "f"],
    4: ["g", "h", "i"],
    5: ["j", "k", "l"],
    6: ["m", "n", "o"],
    7: ["p", "q", "r", "s"],
    8: ["t", "u", "v"],
    9: ["w", "x", "y", "z"]
  }

  const helper = (i, str) => {
    if (i >= digits.length) return str;

    const letters = map[digits[i]];

    for (let letter of letters) {
      const temp = helper(i+1, str+letter);
      if (temp !== undefined) res.push(temp);
    }
  }

  helper(0, "");

  return res;
};
```

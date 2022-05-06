# 2021 KAKAO BLIND RECRUITMENT

## [신규 아이디 추천](https://programmers.co.kr/learn/courses/30/lessons/72410)
- **Input:** a string `new_id`.
- **Output:** return a new string that represents an id suggestion based on `new_id`.
- **Description**
  - Step 1: Convert all alphabets to lowercase.
  - Step 2: Exclude all characters except for lowercase alphabets, digits, `-`, `_`, `.`.
  - Step 3: Replace consecutive `.`s with a single `.`.
  - Step 4: Remove `.` from the beginning and end if it exists.
  - Step 5: If string is empty, set string to be `a`.
  - Step 6-1: If the string length is greater than 15, exclude all characters after the 15th character.
  - Step 6-2: If after 6-1 a `.` is at the end of the string, remove the `.`.
  - Step 7: If the string length is less than 3, fill the string with the last character until it has a length of 3.
### Example
```js
solution("...!@BaT#*..y.abcdefghijklm"); // "bat.y.abcdefghi"
solution("z-+.^."); // "z--"
solution("=.="); // "aaa"
```
### Solution
- Use regular expressions for the most part.
```js
const solution = (new_id) => {
  // Step 1
  new_id = new_id.toLowerCase();
  // Step 2
  new_id = new_id.replace(/[^a-z0-9\-\_\.]/g, "");
  // Step 3
  new_id = new_id.replace(/\.{2,}/g, ".");
  // Step 4
  new_id = new_id.replace(/^\.|\.$/g, "");
  // Step 5
  if (new_id === "") new_id = "a";
  // Step 6-1
  if (new_id.length > 15) new_id = new_id.substring(0, 15);
  // Step 6-2
  new_id = new_id.replace(/\.$/g, "");
  // Step 7
  if (new_id.length < 3) new_id = new_id.padEnd(3, new_id[new_id.length-1]);
  
  return new_id;
}
```

## [메뉴 리뉴얼](https://programmers.co.kr/learn/courses/30/lessons/72411)
- **Inputs**
  - An array of strings `orders` where `orders[i]` represents the menus that a customer had ordered.
    - Ex: `orders[i] = "ABCFG"` means that the customer ordered menus `A`, `B`, `C`, `F`, and `G`.
  - An array of integers `course` where `course[i]` represents the number of menus to include as a course.
    - Ex: `course = [2,3,4]` means courses comprised of two, three, four menus.
- **Description**
  - A chef is devising a course menu based on the menus that have been ordered together most frequently by previous customers.
  - A course menu should comprise of at least two single menus.
  - A combination of single menus is considered as a course menu only if at least two customers had ordered them together.
    - Ex: at least two customers must have each ordered "A" and "C" together for "AC" to be considered as a course menu.
- **Output**
  - Return an array of strings representing the potential course menus.
- **Constraints**
  - `2 <= orders.length <= 20`.
  - `2 <= orders[i].length <= 10`.
    - `orders[i]` is comprised of capitalized alphabets with no duplicates.
  - `1 <= course.length <= 10`.
    - `course` is sorted in ascending order.
  - `2 <= course[i] <= 10`.
    - `course[i]` is a natural number.
### Example
```js
solution(["ABCFG", "AC", "CDE", "ACDE", "BCFG", "ACDEH"], [2,3,4]); // ["AC", "ACDE", "BCFG", "CDE"]
solution(["ABCDE", "AB", "CD", "ADE", "XYZ", "XYZ", "ACD"], [2,3,5]); // ["ACD", "AD", "ADE", "CD", "XYZ"]
solution(["XYZ", "XWY", "WXA"], [2,3,4]); // ["WX", "XY"]
```
### Solution
- For each `course[i]`.
  - Compute all the possible menu combinations (of the given number of menus) from each customer's order.
  - Record the number of occurrences for each menu combination.
  - Pick out the menu combination(s) with the most occurrences that are at least 2.
```js
function solution(orders, course) {
  const courseMenus = [];
  
  for (let numOfMenus of course) {
    // Find out all possible menu combinations (of the given number of menus) for each order.
    const menuCombinations = [];
    for (let order of orders) {
      menuCombinations.push(...getCombinations(order, numOfMenus));
    }
    
    // Record the number of occurrences of each combination.
    const combinationFreq = {};
    for (let menuCombination of menuCombinations) {
      combinationFreq[menuCombination] = ++combinationFreq[menuCombination] || 1;
    }
    
    // Find out the largest number of occurrences of a combination.
    let maxFreq = 0;
    for (let freq of Object.values(combinationFreq)) {
      if (freq > maxFreq) maxFreq = freq;
    }
    // Find out the combination(s) with the largest frequency.
    const mostFreqCombinations = [];
    for (let [combination, freq] of Object.entries(combinationFreq)) {
      if (freq === maxFreq && freq >= 2) mostFreqCombinations.push(combination);
    }
    
    courseMenus.push(...mostFreqCombinations);
  }
  
  return courseMenus.sort();
}

// Returns all possible combinations with a length of n of a given string.
function getCombinations(str, n) {
  const combinations = [];
  
  const helper = (idx, combination) => {
    if (combination.length === n) {
      combinations.push(combination);
      return;
    }
    if (idx >= str.length) return;
    
    helper(idx+1, combination+str[idx]);
    helper(idx+1, combination);
  }
  
  helper(0, "");
  
  // Return each combination in combinations sorted in alphabetical order.
  // Sort each combination in alphabetical order because "BA" should be counted as "AB" when counting occurrences.
  return combinations.map(combination => [...combination].sort().join(""));
}
```

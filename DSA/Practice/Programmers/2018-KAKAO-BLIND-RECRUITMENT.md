# 2018 KAKAO BLIND RECRUITMENT

## [[1차] 비밀지도](https://programmers.co.kr/learn/courses/30/lessons/17681)
- **Inputs**
  - A positive integer `n` representing the length of the side of a square map.
  - An array of positive integers `arr1` representing a map.
    - The binary code of `arr1[i]` represents a row in the map.
    - `arr1[i][j]` represents a coordinate on the map (either 0 or 1).
  - An array of positive integers `arr2` representing a map.
    - The binary code of `arr1[i]` represents a row in the map.
    - `arr1[i][j]` represents a coordinate on the map (either 0 or 1).
- **Description**
  - Overlap the two maps to decipher the secret map.
  - To decipher the map, when the two maps are overlapped, if either coordinate represents a wall, there should be a wall.
- **Output:** return an array of the secret map where `1` is represented by `"#"` and `0` is represented by `" "`.
- **Constraints**
  - `1 <= n <= 16`.
  - `arr1.length = arr2.length = n`.
  - The length of `arr1[i]`'s binary code is equal to or less than `n`.
    - Therefore, `0 <= x <= 2^n - 1`.
### Example
```js
solution(5, [9, 20, 28, 18, 11], [30, 1, 21, 17, 28]); // ["#####","# # #", "### #", "# ##", "#####"]
solution(6, [46, 33, 33 ,22, 31, 50], [27 ,56, 19, 14, 14, 10]); // ["######", "### #", "## ##", " #### ", " #####", "### # "]
```
### Solution
- Use the bit operator OR (`|`).
```js
const solution = (n, arr1, arr2) => {
  const answer = [];
  for (let i = 0; i < n; i++) {
    let binary = (arr1[i] | arr2[i]).toString(2).replace(/1|0/g, (d) => d === "1" ? "#" : " ").padStart(n, " ");
    answer.push(binary);
  }
  return answer;
}
```
```js
const solution = (n, arr1, arr2) => {
  return arr1.map((x,i) => (x | arr2[i]).toString(2).replace(/1|0/g, (d) => d === "1" ? "#" : " ").padStart(n, " "));
}
```

## [[1차] 다트게임](https://programmers.co.kr/learn/courses/30/lessons/17682)
- **Input:** a string `dartResult` representing the point, bonus, [option] of three attempts.
- **Description**
  - Points range from 1 to 10.
  - There are three areas on the dart board (`S`, `D`, `T`).
    - `S`: points<sup>1</sup>
    - `D`: points<sup>2</sup>
    - `T`: points<sup>3</sup>
  - There are two options (`*`, `#`).
    - `*`: doubles the points of the current and the previous attempt.
    - `#`: deduct the current points.
    - Options can overlap with one another.
    - At any given attempt, only one option is applied.
- **Output:** return the total points obtained from the three attempts.
### Example
```js
solution("1S2D*3T"); // 37
solution("1D2S#10S"); // 9
solution("1D2S0T"); // 3
solution("1T2D3D#"); // -4
```
### Solution
```js
const solution = (dartResult) => {
  const darts = dartResult.match(/\d{1,2}[SDT][\*\#]?/g);
  
  const pointsPerAttempt = [];
  
  for (let dart of darts) {
    let points = 0;
    
    const dartArr = dart.split(/([SDT])/);
    
    // Bonus (S,D,T)
    if (dartArr[1] === "S") points += dartArr[0] ** 1;
    else if (dartArr[1] === "D") points += dartArr[0] ** 2;
    else if (dartArr[1] === "T") points += dartArr[0] ** 3;
    
    // Options (*,#)
    if (dartArr[2] === "*") {
      points *= 2;
      // The last element in pointsPerAttempt is the previous attempt of the current attempt.
      if (pointsPerAttempt.length > 0) pointsPerAttempt[pointsPerAttempt.length - 1] *= 2;
    } else if (dartArr[2] === "#") {
      points *= -1;
    }
    
    pointsPerAttempt.push(points);
  }
  
  return pointsPerAttempt.reduce((sum, x) => sum + x, 0);
}
```

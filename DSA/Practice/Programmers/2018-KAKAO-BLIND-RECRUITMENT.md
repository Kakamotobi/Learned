# 2018 KAKAO BLIND RECRUITMENT

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

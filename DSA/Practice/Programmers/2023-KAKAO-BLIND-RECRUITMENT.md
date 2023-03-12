# 2023 KAKAO BLIND RECRUITMENT

## [개인정보 수집 유효기간](https://school.programmers.co.kr/learn/courses/30/lessons/150370)
```js
function solution(today, terms, privacies) {
  const ans = [];

  const termsObj = terms.reduce((acc, curr) => { 
    const [term, monthsToExpire] = curr.split(" ");
    acc[term] = parseInt(monthsToExpire);
    return acc;
  }, {});

  const [tYear, tMonth, tDay] = today.split(".").map(x => parseInt(x));

  privacies.forEach((privacy, idx) => {
    const [dateCollected, term] = privacy.split(" ");
    const [pYear, pMonth, pDay] = dateCollected.split(".").map(x => parseInt(x));

    const diff = [
      tYear - pYear,
      tMonth - pMonth,
      tDay - pDay
    ];

    let numDays = 0;
    numDays += diff[0] * 28 * 12;
    numDays += diff[1] * 28;
    numDays += diff[2];

    if (numDays / 28 >= termsObj[term]) ans.push(idx + 1);
  });

  return ans;
}
```

---
layout: post
author: study
title:  "완전탐색 : 모의고사"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---


# 문제 설명

  수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

  1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
  2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
  3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

  1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

  - 제한 조건
    - 시험은 최대 10,000 문제로 구성되어있습니다.
    - 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
    - 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

 입출력 예
 | participant | return |
 | [1,2,3,4,5] | [1] |
 | [1,3,2,4,2] | [1,2,3] |


# 문제풀이
  단순하다. 순번을 만들어 내고, 해당 순번에 맞는 값이 답과 동일한지 판단하는 것.
  순번을 어떻게 만들어 내느냐가 관건.
  
  내가 해결한 방법은 규칙성을 찾고, 규칙에 맞게끔 순번을 만들어 냈다.
  2번 수포자의 경우 홀수번 2 고정에 짝수번 1,3,4,5 규칙
  3번 수포자의 경우 순번을 2번씩 적용


```javascript
let idx1, idx2=-1, idx3=-1;
let i=0, len=answers.length;

while(i<len) {
    idx1 = i%5;
    if(!(i%2)){
        var tmpIdx2 = (idx2+1)%5;
        if( tmpIdx2 === 1 ){
            idx2 = tmpIdx2+1;
        }
        else {
            idx2 = tmpIdx2;
        }
        idx3 = (idx3+1)%5;
    } 
    i++;
}
```
 이 순번에 따라 값을 비교하면 나온다.

```javascript
let sample1 = [1, 2, 3, 4, 5];
let sample2 = [3, 1, 2, 4, 5];

let count=[0, 0, 0];
let idx2=-1, idx3=-1;

let i=0, len=answers.length;
while(i<len) {
    idx1 = i%5;
    if(!(i%2)){
        var tmpIdx2 = (idx2+1)%5;
        if( tmpIdx2 === 1 ){
            idx2 = tmpIdx2+1;
        }
        else {
            idx2 = tmpIdx2;
        }
        idx3 = (idx3+1)%5;
        if(answers[i] === 2) {
            count[1]++;
        }
    } else {
        if(answers[i] === sample1[idx2]) {
            count[1]++;
        }
    }

    if(answers[i] === sample1[idx1]) {
        count[0]++;
    }
    if(answers[i] === sample2[idx3]) {
        count[2]++;
    }
    i++;
}

var max=0;
count.forEach( i=> {
    if(max <= i) {
        max = i;
    }
});
var result = count.reduce( (acc, cur, idx) => {
    if(cur === max) {
        acc.push(idx+1)
    }
    return acc
}, []);
```

 잘 살펴보면 3번 수포자의 idx는 1, 1, 2, 2, 3, 3, 4, 4, 5, 5 ... 의 순서이고, sample에 매칭이 되어야 비로소 수포자의 순번이 동일하게 된다. (3, 3, 1, 1, 2, 2, 4, 4, 5, 5) 이는 직관적이지 않다.

 찾아보니 직관적으로 풀어낸 답이 존재 하다.


```javascript
let a1 = [1, 2, 3, 4, 5];
let a2 = [2, 1, 2, 3, 2, 4, 2, 5];
let a3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];

let lenA1 = a1.length;
let lenA2 = a2.length;
let lenA3 = a3.length;

let a1c = answers.filter( (v,idx) => v === a1[idx%lenA1]).length;
let a2c = answers.filter( (v,idx) => v === a2[idx%lenA2]).length;
let a3c = answers.filter( (v,idx) => v === a3[idx%lenA3]).length;
```
 filter를 통해 주어진 배열에서 조건에 맞는 값을 뺴낸다.
 그러면 a1c, a2c, a3c 은 length로 인해 답을 Count 하게 되며 해당 값의 max를 구해 max와 값이 같으면 순서대로 답에 push 해주면 된다.

 ```javascript
function solution(answers) {
  var answer = [];
  var a1 = [1, 2, 3, 4, 5];
  var a2 = [2, 1, 2, 3, 2, 4, 2, 5]
  var a3 = [ 3, 3, 1, 1, 2, 2, 4, 4, 5, 5];

  let lenA1 = a1.length;
  let lenA2 = a2.length;
  let lenA3 = a3.length;

  var a1c = answers.filter((a,i)=> a === a1[i%lenA1]).length;
  var a2c = answers.filter((a,i)=> a === a2[i%lenA2]).length;
  var a3c = answers.filter((a,i)=> a === a3[i%lenA3]).length;
  var max = Math.max(a1c,a2c,a3c);

  if (a1c === max) {answer.push(1)};
  if (a2c === max) {answer.push(2)};
  if (a3c === max) {answer.push(3)};

  return answer;
}

 ```
 상당히 직관적이다. 이렇게 문제해결하자.
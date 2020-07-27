---
layout: post
author: study
title:  "연습문제 : 2 x n 타일링"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---
category: 

 url: https://programmers.co.kr/learn/courses/30/lessons/12900#


# 문제풀이
  문제가 동적 프로그래밍을 보기 위해서 만든 문제이다.
  그렇다 보니, return 값이 너무나도 커져야 하는 문제가 존재하는데,
  이를 해결하기 위한 방법을 제대로 이해하지 못했다.

  물론 그전에 동적 프로그래밍인지를 아는 것도 수학적인 문제라 쉽게 파악하지 못했다.

  이 문제를 풀기 위해선
  1. 타일이 놓일 조건이 가로일 때 1개, 세로일 떄 2개가 깔릴 경우 각각의 경우의 수를 깔리지 않을 값에 더해주는 점화식을 이끌어 낼수 있느냐.
     `n = [n-1] + [n-2]`
  2. 각각의 조합을 구해보면, 파스칼 삼각형을 통해 피보나치 수열임을 이끌어 낼수 있느냐.
  
  두가지로 나뉠 수 있다.
  올림피아드 문제를 그냥 프로그래밍으로 옮겨 놓은 듯한 문제. 
  따지고 보면 결국 피보나치 동적 프로그래밍을 할수 있느냐인데, 이는 동적 프로그래밍을 배우면 가장 먼저 알게되는 사항이라, 솔직히 상당히 별로인 문제.

  그런데 거기서 끝나지 않는다!

  문제의 효율성을 파악하기 위해 커져버린 return 값을 해결하기 위해, 출제자는 동적 프로그래밍으로만 풀도록 제한 해야 하는 만큼, limit값이 넘어버리면 다음 피보나치 값은 제한이 걸린 채로 움직인다.

  만일 각각의 조합을 직접 구하고 마지막 return에만 제한을 걸어버리면 답이 다르게 된다. 

  여러모로 신경써야 할 부분이 많았던 문제이다.


```javascript
function solution1(n) {
    let arr = [1, 2];
    for (let i = 2; i < n; i++) {
        arr[i] = (arr[i - 1] + arr[i - 2])%1000000007; // 제한이 걸린채 값이 정해진다.
    }
    return arr[n-1];
}
```

```javascript
function solution(n) {
    var answer = 0;
    let twoCount = 0;
    let oneCount = n;
    answer += getCombination(oneCount, twoCount);
    while(n>1) {
        n -= 2;
        oneCount -= 2;
        twoCount++;
        answer += getCombination(oneCount, twoCount);
    }
    
    return answer%1000000007; //마지막 값에서만 제한을 걸다 보니, 값이 다르다. 그렇다고 아래 total값에 제한을 걸기도 애매하다.
}

function getCombination(oneCount, twoCount) {
    if(oneCount === 0 || twoCount === 0) {
        return 1;
    }
    let total = oneCount+twoCount;
    let top = 1;
    let bottom  = 1;
    
    for(let i=0; i<total; i++) {
        top *= total-i;
    }
    for(let j=0; j<oneCount; j++) {
        bottom *= oneCount-j;
    }
    for(let k=0; k<twoCount; k++) {
        bottom *= twoCount-k;
    }
    return top/bottom
}
```
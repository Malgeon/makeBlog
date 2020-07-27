---
layout: post
author: study
title:  "연습문제 : 소수찾기"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---
category: 

# 문제 설명

 1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요.

  소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.
  (1은 소수가 아닙니다.)
 
 - 제한사항
  - n은 2이상 1000000이하의 자연수입니다.
  

 입출력 예
 | n | result |
 | --- | --- |
 | 10 | 4 |
 | 5 | 3 |
 


# 문제풀이
  단순 수학 문제이다.
  수학시간에 배운 어떤 체... 를 이용하면 쉽게 해결할 수 있다.

```javascript
    let answer = 0;
    let arr = [];

    for (let i = 2; i <= n; i++) {
        arr[i] = i; // 순서를 맞추기 위함. 2번자리에 2가 들어가게끔 조정.
    }

    for (let i = 2; i <= n; i++) {
        if (arr[i] === 0) 
            continue;

        for (let j = i + i; j <= n; j += i) {
            arr[j] = 0;
        }
    }    
    return arr.filter( i => i !== 0).length;
```


# (번외) 소수판단
```javascript
function isPrimeNumber(number) {
  const notPrime = [0, 1];
  if (notPrime.includes(number)) return false;

  for (let i = 2; i * i <= number; i++) { 
      //number의 제곱근만 판단해도 소수판단 가능
    if (number % i === 0) return false;
  }
  return true;
}
```
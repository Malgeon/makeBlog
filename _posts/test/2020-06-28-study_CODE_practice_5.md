---
layout: post
author: test
title:  "연습문제 : 카펫"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---
category: 

 url: https://programmers.co.kr/learn/courses/30/lessons/42842?language=javascript


# 문제풀이
  우선 yellow의 약수를 x, y라고 하자.
  제한조건에서 brown이 정확하게 들어온다고 가정할 시, brwon은 2(x+y)+4가 될 것이다. 이때 주어진 brown과 값이 같다면 끝.
  
  테스트 케이스에 yellow가 1개인 경우가 있어서 다행히 yellow에 +1를 할 수 있었다. 없을 경우도 생각해서 제출할 때, 테스크 케이스를 확인하고 제출하자.
  

```javascript
function solution(brown, yellow) {
    var answer = [];
    let x, y;
    for(let i=1; i<yellow+1; i++) {
        if(yellow%i === 0 ) {
            x = i;
            y = yellow/i;
            var brownNum = 2*(x+y)+4;
            if(brownNum === brown) {
                break;
            }
        }
    }
    answer.push(y+2);
    answer.push(x+2);
    return answer;
}
```

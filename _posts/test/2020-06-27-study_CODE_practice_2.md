---
layout: post
author: test
title:  "연습문제 : 124 나라의 숫자"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---
category: 

 url: https://programmers.co.kr/learn/courses/30/lessons/12899


# 문제풀이
  규칙성을 찾고, 해당 규칙을 구현할 수 있는지 알아보는 문제.
  규칙을 구현하기에 걸림이 되었던 부분은 0이 존재하지 않으며, 3진수이지만 자릿수가 올라 갈 경우 올라가지 않고 4가 붙는다는 사실이다.

  해당 경우에 대한 대처를 해 놓은다면 문제 풀기는 수월했다.

```javascript
   function solution(n) {
    var answer = '';
    let value = n;

    while(value>3) {
        var div = parseInt(value/3);
        var rest = value%3;

        if(rest === 0) {
            value = div-1;
            answer = '4' + answer; //먼저 오게 함으로써 자릿수 보정
        } else {
            value = div;
            answer = rest + answer;
        }
    }

    if(value === 3) {
        answer = '4' + answer;
    } else {
        answer = value + answer;
    }
    return answer;
}
```

---
layout: post
author: study
title:  "연습문제 : 멀쩡한 사각형"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---
category: 

 url: https://programmers.co.kr/learn/courses/30/lessons/62048


# 문제풀이
  수학문제를 풀어야 한다. 대각선이 지나는 단위 정사각형.
  물론 조금만 더 생각해 보면, 수학적 사고력으로 해결할 수 있는 문제이나, 제한시간이 주어지고 갑자기 수학적 사고를 하도록 한다면 문제를 해결할 수 있을지 부정적이다. 마지막으로 미뤄두고 풀어보도록 해야지.

  우선 서로소인 사각형의 대각선을 생각해 보자.
  서로소인 관계를 설정하는 이유는, 그렇지 않을 때 대각선은 나뉘어지는 부분이 있기 때문.
  예를들어 5X4인 사각형(서로소인 관계의 사각형)의 대각선이 칠해진 개수는 

    □□□■■
    □□■■□
    □■■□□
    ■■□□□

    □□□□■
    □□□■■
    □□■■□
    ■■■□□ 

    □□□□■
    □□□□■
    □□□■■
    ■■■■□

    □□□□■
    □□□□■
    □□□□■
    ■■■■■

    5+4-1 = 8개

    가로 + 세로 -1 이 대각선에 의해 칠해진 개수가 된다.

  m*n 사각형에서 m과 n의 최대공약수가 a라고 할 떄,
  m = ab, n = ac 라고 할 수 있다.(b와 c는 서로소)
  그렇게 되면 m*n의 사각형의 넓이는 a^2*bc이며 
  이는 b*c의 사각형이 a^2개가 있다고 생각할 수 있다.

  이때 잘 생각해 보면 대각선이 지나가는 b*c의 사각형은 a개가 될수 있다.
  따라서 b*c의 대각선에 의해 칠해진 사각형 개수는 b+c-1
  m*n의 사각형의 경우 a(b+c-1) = ab+ac-a = 가로 + 세로 - 가로와 세로의 최대공약수 

  이렇게 해결.


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

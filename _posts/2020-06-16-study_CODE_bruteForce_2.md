---
layout: post
author: study
title:  "완전탐색 : 소수 찾기"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---


# 문제 설명

  한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

  각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

  - 제한 조건
    - numbers는 길이 1 이상 7 이하인 문자열입니다.
    - numbers는 0~9까지 숫자만으로 이루어져 있습니다.
    - 013은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

 입출력 예
 | participant | return |
 | "17" | 3 |
 | "011" | 2 |


# 문제풀이
  완전히 Combination 문제이다. 
 
---
layout: post
author: study
title:  "이분탐색 : 예산"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---


# 문제 설명

  국가의 역할 중 하나는 여러 지방의 예산요청을 심사하여 국가의 예산을 분배하는 것입니다. 국가예산의 총액은 미리 정해져 있어서 모든 예산요청을 배정해 주기는 어려울 수도 있습니다. 그래서 정해진 총액 이하에서 가능한 한 최대의 총 예산을 다음과 같은 방법으로 배정합니다.

 1. 모든 요청이 배정될 수 있는 경우에는 요청한 금액을 그대로 배정합니다.
 2. 모든 요청이 배정될 수 없는 경우에는 특정한 정수 상한액을 계산하여 그 이상인 예산요청에는 모두 상한액을 배정합니다. 
   상한액 이하의 예산요청에 대해서는 요청한 금액을 그대로 배정합니다. 

 입출력 예
 | budgets | M | return |
 | --- | --- | --- |
 |[120, 110, 140, 150] | 485 | 127 |

# 문제풀이
  단순히 BST를 사용해서 문제를 해결하는 것이 아닌 BST의 컨셉을 이용해 가장 근사치 값을 찾는 것이다. (수치해석..)
 계산적으로 budgets의 상한값의 MAX값은 150이며, MIN 값은 0이다.
  이를 시작점을 삼아 middle 값을 정해서, result를 계산하고 해당 result 값과 근사치를 비교하여 
 크면 middle point를 right로, 작으면 middle point를 left로 기준을 잡고 다시 middle 값을 구한다. (가장 근사치가 나오는 left가 right를 넘어서는 직전 종료)

 따라서 풀이는 단순하다.


  ```javascript
  function solution(budgets, M) {
      
    let left = 0;
    let right = 0;
    var answer = 0;
    
    budgets.forEach( (i) => {
      if(i>right) {
        right = i;
      }
    })

    let middle = 0;

    while (left <= right) {
      let sum = 0;
      middle = parseInt((left + right)/2);

      budgets.forEach( (i) => {
        if (i >= middle) {
          sum += middle;
        } else {
          sum += i;
        }
      })

      if (sum > M) {
        right = middle-1;
      }
      else {
        answer = middle;
        left = middle+1;
      }
    }
      return answer;
  }
  ```
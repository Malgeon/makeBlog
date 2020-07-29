---
layout: post
author: test
title:  "정렬 : 가장 큰 수"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---


# 문제 설명

  0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

  예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

  0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

  - 제한 사항
    - numbers의 길이는 1 이상 100,000 이하입니다.
    - numbers의 원소는 0 이상 1,000 이하입니다.
    - 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

 입출력 예
 | participant | return |
 | --- | --- |
 | [6, 10, 2] | 6210 |
 | [3, 30, 34, 5, 9] | 9534330 |


# 문제풀이
  정렬의 기준을 어떻게 효과적으로 생각할 수 있는 여부를 판단하는 문제인거 같다. 
  
  내가 처음 생각한 기준은, 모든 값을 전체적으로 동일하게 규정을 해버리는 것이었다. 
  모든 숫자를 끝의 숫자를 증가하는 방식을 이용해 자릿수를 맞춤으로써 값이 동일하게 규정되었다고 판단했다.

```javascript
  var data = numbers.map( i => {
	let changeData = String(i);
    let point;
    for(let j=1; j<3; j++) {
        if(changeData[j] == undefined) { 
            point = changeData[j-1];
            changeData = changeData.concat(point);
        }
    }
    return changeData;
})
```
 하지만 30, 301을 기준으로 하면 30 -> 300, 301 
   ==> 30130 < 30301 으로써 동일하게 규정을 할 수가 없음을 확인 하였다. 

   동일한 규정을 찾으려면 너무 끝도 없었고 차라리 다른 기준을 생각해야 했다. 찾아본 결과 1:1 정렬을 하되 기준을 a와 b가 아닌 ab, ba로 삼아 진행하면 해결이 되었다.

```javascript
  var data = numbers.map( i => i+'').sort((a,b)=> (b+a)-(a+b));
```

데이터에서 0000이 나올수 있으니 방지 차원에서
```javascript
  return data[0] === '0' ? '0' : data;
```
  이번 문제를 통해 본 정렬 문제 테크닉은 최대한 작게 볼수 있어야 하는 것 같다.
  
---
layout: post
author: study
title: "완전탐색 : 문자열 압축"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---
category: 2020 KAKAO BLIND RECRUITMENT


# 문제 설명

 데이터 처리 전문가가 되고 싶은 어피치는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
 
 간단한 예로 aabbaccc의 경우 2a2ba3c(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, abcabcdede와 같은 문자열은 전혀 압축되지 않습니다. 어피치는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.

 예를 들어, ababcdcdababcdcd의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 2ab2cd2ab2cd로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 2ababcdcd로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.

 다른 예로, abcabcdede와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 abcabc2de가 되지만, 3개 단위로 자른다면 2abcdede가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

 압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

 - 제한사항
  - s의 길이는 1 이상 1,000 이하입니다.
  - s는 알파벳 소문자로만 이루어져 있습니다.


 - 입출력 예
 | s | result |
 | "aabbaccc" | 7 |
 | "ababcdcdababcdcd" | 9 |
 | "abcabcdede" | 8 |
 | "abcabcabcabcdededededede" | 14 |
 | "xababcdcdababcdcd" | 17 |
 
 

# 문제풀이
  
  문자열을 비교한 뒤 압축 여부를 판단하고, 가능 하다면 압축 뒤 결과 글자를 비교하여 가장 작은 값을 return 하면 된다.
  
  압축 여부를 판단하는 것이 중요한데, 압축은 모두 동일하게 적용되어야 하며, 제일 앞에서 부터 차례로 적용된다. 
  즉, 문자열을 일정한 기준으로 자르고, 앞문자와 뒷문자를 비교하여 같으면 압축을 할수 있다는 의미이다.

  압축은 문자열의 1/2부터 가능하며, 모든 값을 해 본 뒤 가장 작은 값을 구하는 방법으로 진행하였다.

  정답률 25.9%가 level 2 라니.. 이상하다.



```javascript
function solution(s) {
    let answer = 0;
    let strLen = s.length; //여러번 쓰이기 때문에 글자수 길이를 따로 저장해준다.

    if(strLen == 1) { //중복 길이를 제외
        return 1;
    }
    for (let i = 1, len = parseInt(strLen / 2); i <= len; i++) { 
        let zipSet = makeZip(s, i);
        let restCharNum = strLen - zipSet.reduce((num, cur) => {
            num += cur * i;
            return num;
        }, 0);
        let zipCharNum = restCharNum + zipSet.map(i => i + '').reduce((num, cur) => {
            num += cur.length + i; //여기서 만일 압축숫자가 단위수가 증가하면 그만큼 글자수도 증가한다는것을 놓쳐서 꽤 많애 헤멨다. 단순한걸 놓치면 답을 찾기가 어렵다.
            return num;
        }, 0)

        if (answer === 0) {
            answer = zipCharNum;
        } else if (answer > zipCharNum) {
            answer = zipCharNum;
        }
    }

    return answer;
}


function makeZip(str, sliceNum) {
    let len = Math.ceil(str.length / sliceNum);
    let start = 0;
    let target;
    let prevTarget = -1;
    let strCount = 0;
    let strFlag = []; //처음엔 맵으로 저장을 했는데, 규칙상 바로 오지않으면 압축이 되지 않는다는 것을 이해하지 못했다. 처음 헤멘 포인트.
    let flag = false;

    let sliceStr = str;

    while (start < len) {
        prevTarget = target;
        target = sliceStr.substring(0, sliceNum);

        if (target.length < sliceNum) {
            break;
        }
        if (prevTarget === target) {
            if (flag) {
                strFlag.push(2)
                strCount++;
                flag = false;
            } else {
                strFlag[strCount - 1]++;
            }
        } else {
            flag = true;
        }

        sliceStr = sliceStr.slice(sliceNum);
        start++;
    }
    return strFlag;
}
```


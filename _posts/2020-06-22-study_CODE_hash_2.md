---
layout: post
author: study
title:  "해시 : 완주하지 못한 선수"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---


# 문제 설명

 수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
 마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
 
 - 제한사항
  - 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
  - completion의 길이는 participant의 길이보다 1 작습니다.
  - 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
  - 참가자 중에는 동명이인이 있을 수 있습니다.

 입출력 예
 | participant | completion | return |
 | --- | --- | --- |
 | [leo, kiki, eden] | [eden, kiki] | leo |
 | [marina, josipa, nikola, vinko, filipa] | [josipa, filipa, marina, nikola] | vinko |
 | [mislav, stanko, mislav, ana] | [stanko, ana, mislav] | mislav |


# 문제풀이
  
  해시를 이용해 풀수 있다.
  각 data를 Key, data의 개수를 value로 HashMap을 만들어
  중복 시 map의 값을 줄여나가며, 0 일경우 지운다.
  Key들의 집합이 답이겠으나,
  들어오지 못한 인원은 1명이라는 제한 조건으로 인해 단순히 key를 return 하면 된다.

  해시를 사용하는 방법은 아래와 같이 2가지이다.

   1. Map()을 이용한 방법.
   2. 객체를 사용한 방법

1. 
```javascript
function solution(participant, completion) {
  var answer = '';
  let comMap = new Map();

  participant.forEach( (i) => {
    var value = comMap.get(i);
    if((value == undefined)) {
      comMap.set(i, 1);
    } else {
      value += 1;
      comMap.set(i, value);
    }
  })

  completion.forEach( (i) => {
    var value = comMap.get(i);
    value -= 1;
    if (value === 0 ) {
      comMap.delete(i);
    }
    else {
      comMap.set(i, value);
    }
  })

  for(let key of comMap.keys()) {
    answer = key;
  }

  return answer;
}
```

2. 객체를 사용하는 방법이다. 

```javascript
function solution(participant, completion) {
  return participant.find( i => !completion[i]--, completion.map( i=> completion[i] = (completion[i]|0)+1));
}
```

 하나씩 이해해보자.
 
 전체적인 매커니즘은 다음과 같다.
 completion에 배열의 값을 프로퍼티로 하는 key, 해당 멤버의 값을 value로 하여 값을 추가한다.
 participant의 값을 통해 completion 프로퍼티로 조사하여 value를 줄여가며, 해당 프로퍼티의 value의 값이 =< 0 또는 undefine 값을 찾아내는 것이다.

  1. arr.map()
    map을 통해 completion의 배열 값으로 하는 프로퍼티를 생성하며, 해당 프로퍼티의 값을 `0과 or 처리` 함으로써 값의 존재유무를 판단한다. (없다면 undefined|0 ==> 0, 있다면 숫자|0 ==> 숫자 Wow!)
  2. arr.find(callback[, thisArg])
    제한조건상 1개만이 return 값이기 때문에 find 사용. reduce로 대체 가능
    callback 함수에 thisArg값으로 1번에서 만든 값을 넣어 줌으로써 하나씩 찾아보도록 한다. 
      정확환 순서는 (!completion[i]) 값을 찾고 해당값을 -- 하고 넘겨준다.

 map과 객체리터럴이 꽤 강력함을 여기서 확인한다.

 또 함수형 프로그래밍으로 매개변수만 받도록 하면 아래와 같다.

```javascript
var solution=(_,$)=>_.find(_=>!$[_]--,$.map(_=>$[_]=($[_]|0)+1))
```
 !?
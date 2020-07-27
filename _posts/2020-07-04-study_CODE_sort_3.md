---
layout: post
author: study
title:  "정렬 : 튜플"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---
category: 2019 카카오 개발자 겨울 인턴십

# 문제 설명

  셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

  (a1, a2, a3, ..., an)
  튜플은 다음과 같은 성질을 가지고 있습니다.

  중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
  원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다. ex : (1, 2, 3) ≠ (1, 3, 2)
  튜플의 원소 개수는 유한합니다.
  원소의 개수가 n개이고, 중복되는 원소가 없는 튜플 (a1, a2, a3, ..., an)이 주어질 때(단, a1, a2, ..., an은 자연수), 이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.
 
  예를 들어 튜플이 (2, 1, 3, 4)인 경우 이는

  와 같이 표현할 수 있습니다. 이때, 집합은 원소의 순서가 바뀌어도 상관없으므로


  는 모두 같은 튜플 (2, 1, 3, 4)를 나타냅니다.

  특정 튜플을 표현하는 집합이 담긴 문자열 s가 매개변수로 주어질 때, s가 표현하는 튜플을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

  - 제한사항
   - s의 길이는 5 이상 1,000,000 이하입니다.
   - s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
   - 숫자가 0으로 시작하는 경우는 없습니다.
   - s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
   - s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
   - return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.


# 문제풀이
  제한조건으로 부터 문제해결방향을 잘 파악해야 하는 것 같다.
  여기서 중요하게 생각해야 할 제한 조건은 1. 주어진 s가 가리키는 튜플은 중복되는 원소가 없다. 2.주어진 s는 하나의 튜플을 올바르게 표현한다.

  그렇다면 s를 배열로 바꾸고, 배열의 크기에 따라 정렬한 뒤 Set에 넣어주면 해결
  
   해결 난항 요소 - sort()에서 length 간 비교가 가능하다. 매개변수가 없을시 문자열 비교이다.( 문자열 비교시 : 10 < 2 )

```javascript
function solution(s) {
    var answer = [];
    let len = s.length;
    const caliValue = s.substr(2,len-4);

    let tupleSet = caliValue.split("},{").reduce( (list,item) => {
        list.push(item.split(","));
        return list;
    }, []).sort((a,b) => a.length - b.length);

    var tuple = new Set();
    tupleSet.forEach( i => {
        i.forEach( k => {
            tuple.add(Number(k));
        })
    });

    answer = [...tuple];
    return answer;
}
```
# 더 좋은 풀이
  배열로 바꿀수 있는 문자열을 바꾸고자 할떄, 조건을 잘 살펴봐야 한다.
  여기선 객체 문자열로 바꿀수 있다.
  replace를 사용하여 객체 문자열로 바꿔준 다음 해당 data를 JSON.parse에 집어넣으면 배열로 완성된다.

```javascript
function solution(s) {
    var answer = [];

    let tupleSet = JSON.parse(s.replace(/{/g, '[').replace(/}/g, ']'))
    .sort((a, b) => a.length - b.length);

    var tuple = new Set();
    tupleSet.forEach( i => {
        i.forEach( k => {
            tuple.add(Number(k));
        })
    });

    answer = [...tuple];
    return answer;
}
```
---
layout: post
author: test
title:  "정렬 : H-Index"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---


# 문제 설명

  H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

  어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이 이 과학자의 H-Index입니다.

  어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

  - 제한사항
    - 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
    - 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

 입출력 예
 | participant | return |
 | --- | --- |
 | [3, 0, 6, 1, 5] | 3 |


# 문제풀이
  정렬은 수학적 사고력이 더 중요한것 같다. 수능 수학 푼 느낌.

  문제 해결을 위한 조건 
   1. h는 결국에는 논문 개수.
   2. 생각해 볼때, 논문 인용수가 가장 많은 순서대로 논문 개수를 늘려나가며, 개수보다 논문 인용수가 적어지는 때 그때 의 값의 H가 된다. (논문 인용수가 아무리 많더라도 하나밖에 없다면 h는 1.)

  
```javascript
function solution(citations) {
  citations = citations.sort( (a,b) => b-a);

  let idx = 0;
  while( idx+1 <= citations[idx] ){
    idx++;
  }
  return idx;
}

```
 정렬 문제는 이런 식이 많을 것 같다. 이래서 이산수학을 배우는 듯.
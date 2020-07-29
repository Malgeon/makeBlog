---
layout: post
author: test
title:  "해시충돌문제 : 호텔 방 배정"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---
category: 2019 카카오 개발자 겨울 인턴십

# 문제 4

# 문제 설명

 정확성 & 효율성 테스트

  스노우타운에서 호텔을 운영하고 있는 스카피는 호텔에 투숙하려는 고객들에게 방을 배정하려 합니다. 호텔에는 방이 총 k개 있으며, 각각의 방은 1번부터 k번까지 번호로 구분하고 있습니다. 처음에는 모든 방이 비어 있으며 스카피는 다음과 같은 규칙에 따라 고객에게 방을 배정하려고 합니다.

  한 번에 한 명씩 신청한 순서대로 방을 배정합니다.
  고객은 투숙하기 원하는 방 번호를 제출합니다.
  고객이 원하는 방이 비어 있다면 즉시 배정합니다.
  고객이 원하는 방이 이미 배정되어 있으면 원하는 방보다 번호가 크면서 비어있는 방 중 가장 번호가 작은 방을 배정합니다.
  예를 들어, 방이 총 10개이고, 고객들이 원하는 방 번호가 순서대로 [1, 3, 4, 1, 3, 1] 일 경우 다음과 같이 방을 배정받게 됩니다.

  원하는 방 번호	배정된 방 번호
      1	1
      3	3
      4	4
      1	2
      3	5
      1	6
  
  전체 방 개수 k와 고객들이 원하는 방 번호가 순서대로 들어있는 배열 room_number가 매개변수로 주어질 때, 각 고객에게 배정되는 방 번호를 순서대로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

  - 제한사항
    - k는 1 이상 1012 이하인 자연수입니다.
    - room_number 배열의 크기는 1 이상 200,000 이하입니다.
    - room_number 배열 각 원소들의 값은 1 이상 k 이하인 자연수입니다.
    - room_number 배열은 모든 고객이 방을 배정받을 수 있는 경우만 입력으로 주어집니다.
    - 예를 들어, k = 5, room_number = [5, 5] 와 같은 경우는 방을 배정받지 못하는 고객이 발생하므로 이런 경우는 입력으로 주어지지 않습니다.


# 문제풀이
  해시를 이용해 값을 저장할 때, 충돌 문제를 얼마나 효율적으로(빠르면서, 공간을 적게쓰도록)
  
  생각보다 재귀함수는 느리지 않았다. (어차피 for문을 도는 거라서 도는 거만큼 함수를 return 하니까.)
  더욱 빠르게 하기 위해 추가로 설정하면 설정한 만큼 동작을 더해야하기 때문에 가장 최소화 하는게 좋다.
  그리고 공간 이슈는 [재귀 함수 발생 수 + flag 배열 생성시 차지하는 공간] 을 중점적으로 살펴봐야 한다. 


  1. 1차 시도 - 정확도 100%, 속도 100%, 공간활용 0%
    해시 충돌 문제 해결에서 배운 단순히 flag 설정하여 하나씩 찾아가며 해결했다.
    최대속도는 1000ms 최소속도는 10ms 편차가 큰 편

```javascript
function solution(k, room_number) {
    var answer = [];
    let used = new Array(k+1).fill(false);

    function searchRoom(idx) {
        debugger;
        if(idx === used.length){
            return;
        }
        if(!used[idx]) {
            used[idx] = true;
            answer.push(idx);
        } else {
            searchRoom(idx+1);
        }
        return;
    }
    
    room_number.forEach( i => {
        searchRoom(i);
    })
    return answer;
}
```

2. 재귀를 이용하지 않고 for문을 통해 다음 index를 찾아 문제 해결 
   시간 초과... 

```javascript
function solution(k, room_number) {
    var answer = [];
    let used = new Array(k+1).fill(false);

    function searchRoom(idx) {
        if(idx === used.length){
            return;
        }
        if(!used[idx]) {
            used[idx] = true;
            answer.push(idx);
        } else {
            var isNext = used.indexOf(false,idx+1)
            if(isNext>0){
                used[isNext] = true;
                answer.push(isNext);
            }
            else {
                return;
            }
        }
        return;
    }
    
    room_number.forEach( i => {
        searchRoom(i);
    })
    return answer;
}
```

3. 재귀를 이용, 찾을때 만 1을 더해주는 것이 아닌 찾지 못할때도 1을 더해준다.
 속도와 공간활용에서 향상되긴하는데, 이것밖에 생각 못했나.. 라고 생각이 드는 답안.

```javascript
function solution(k, room_number) {
    var answer = [];
    let used = new Array(k+1).fill(-1);

    function searchRoom(idx) {
        debugger;
        if(idx === used.length){
            return;
        }
        if(used[idx]<0) {
            used[idx] = idx+1;
            answer.push(idx);
        } else {
            searchRoom(used[idx]);
            used[idx]++;
        }
        return;
    }
    
    room_number.forEach( i => {
        searchRoom(i);
    })

    return answer;
}
```

 4. 해결  찾을 경우 : 처음 노드의 값이 찾은 노드 다음 위치저장
          찾지 못할 경우 :  직전 노드의 값을 다음 노드가 가리키는 위치로 저장.
      또한, 위치 저장값은 해시로 저장.

```javascript
function solution(k, room_number) {
    var answer = [];
    let testUsed = new Map();

    function searchRoom(idx) {
        if (!testUsed.has(idx)) {
            testUsed.set(idx, idx + 1);
            answer.push(idx);
        } else {
            var target = testUsed.get(idx);
            while (target <= k) {
                if (!testUsed.has(target)) {
                    testUsed.set(target, target+1);
                    testUsed.set(idx, target+1);
                    answer.push(target);
                    return;
                } else {
                    var nextTarget = testUsed.get(target);
                    testUsed.set(target, nextTarget+1);
                    target = nextTarget;
                }
            }
        }
    }
    room_number.forEach( i => {
        searchRoom(i);
    })
    return answer;
}
```

# 더 나은 문제풀이
  위의 해결 방법에서는 값에 따른 속도의 편차가 일정하지 않았다. 아래의 방법은 비교적 일정했다.
  재귀로 이동하며 값을 찾아나갈 때, 재귀함수의 return 값이 위치 값을 줌으로써 확인했던 모든 노드의 값을 최종 찾은 값의 다음값으로 저장해준다. (이런 방법이!)

```javascript
const solution = (k, roomNumbers) => {
  const used = new Map();
  return roomNumbers.map((number) => findRoom(number, used));
};

function findRoom(room, used) {
  if (used.get(room) == undefined) {
    used.set(room, room + 1);
    return room;
  }
  let next = findRoom(used.get(room), used);
  used.set(room, next + 1);
  return next;
}
```
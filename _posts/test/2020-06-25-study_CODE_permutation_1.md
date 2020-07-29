---
layout: post
author: test
title: "순열 : 외벽 점검"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---
category: 2020 KAKAO BLIND RECRUITMENT

# 문제 설명

 레스토랑을 운영하고 있는 스카피는 레스토랑 내부가 너무 낡아 친구들과 함께 직접 리모델링 하기로 했습니다. 레스토랑이 있는 곳은 스노우타운으로 매우 추운 지역이어서 내부 공사를 하는 도중에 주기적으로 외벽의 상태를 점검해야 할 필요가 있습니다.

 레스토랑의 구조는 완전히 동그란 모양이고 외벽의 총 둘레는 n미터이며, 외벽의 몇몇 지점은 추위가 심할 경우 손상될 수도 있는 취약한 지점들이 있습니다. 따라서 내부 공사 도중에도 외벽의 취약 지점들이 손상되지 않았는 지, 주기적으로 친구들을 보내서 점검을 하기로 했습니다. 다만, 빠른 공사 진행을 위해 점검 시간을 1시간으로 제한했습니다. 친구들이 1시간 동안 이동할 수 있는 거리는 제각각이기 때문에, 최소한의 친구들을 투입해 취약 지점을 점검하고 나머지 친구들은 내부 공사를 돕도록 하려고 합니다. 편의 상 레스토랑의 정북 방향 지점을 0으로 나타내며, 취약 지점의 위치는 정북 방향 지점으로부터 시계 방향으로 떨어진 거리로 나타냅니다. 또, 친구들은 출발 지점부터 시계, 혹은 반시계 방향으로 외벽을 따라서만 이동합니다.

 외벽의 길이 n, 취약 지점의 위치가 담긴 배열 weak, 각 친구가 1시간 동안 이동할 수 있는 거리가 담긴 배열 dist가 매개변수로 주어질 때, 취약 지점을 점검하기 위해 보내야 하는 친구 수의 최소값을 return 하도록 solution 함수를 완성해주세요.

 - 제한사항
  - n은 1 이상 200 이하인 자연수입니다.
  - weak의 길이는 1 이상 15 이하입니다.
  - 서로 다른 두 취약점의 위치가 같은 경우는 주어지지 않습니다.
  - 취약 지점의 위치는 오름차순으로 정렬되어 주어집니다.
  - weak의 원소는 0 이상 n - 1 이하인 정수입니다.
  - dist의 길이는 1 이상 8 이하입니다.
  - dist의 원소는 1 이상 100 이하인 자연수입니다.
  - 친구들을 모두 투입해도 취약 지점을 전부 점검할 수 없는 경우에는 -1을 return 해주세요.

 - 입출력 예
 | n | weak | dist | result |
 | 12 | [1, 5, 6, 10] | [1, 2, 3, 4] | 2 |
 | 12 | [1, 3, 4, 9, 10] | [3, 5, 7] | 1 |

 
# 문제풀이
  
  문제의 해결 방안을 전혀 생각하지 못하였다. 
  dist가 큰 값부터 하는 방법으로 생각을 해봤지만, 도저히 방도가 생각이 안났다.
  이런 경우는 대부분 완전탐색이라는 것을 기억하자.

  우선 dist가 오는 순서를 정할수 있으며, 해당 순서에 맞게 weak값을 지정하는 방법으로 문제를 풀어 나갈 수 있다.

  dist의 순서는 dist 전체 값으로 하는 순열으로 만들수 있다.

  그러면 해결 가능하다.


```javascript
var n = 12;
var weak = [1, 3, 4, 9, 10]; //[1, 3, 4, 9, 10]
var dist = 	[3, 5, 7]; //[3, 5, 7]

var test = solution(n, weak, dist);

function solution(n, weak, dist) {
  const distLen = dist.length;
  const distSet = permutator(dist, distLen);
  let testWeak = weak.slice();
  const weakLen = weak.length;
  let count = -1;
  
  for(let i=0; i<weakLen; i++) {
    distSet.forEach( item => {
      var result = check(item, testWeak);
      if (result > 0) {
        if(count<0) {
          count = result
        } else{
          if(count > result) {
            count = result;
          }
        }
      }
    })
    let change = testWeak.splice(0,1);
    testWeak.splice(weakLen, 0, change[0]+n);
  }
  
  return count;
}

function check(item, weak) {
  let weakIdx = 0;
  let weakLen = weak.length;
  let count = 0;
  for(let i=0, len=item.length; i<len; i++) {
    var canRepair = item[i] + weak[weakIdx];
    count++;
    weakIdx++;
    while(weakIdx<weakLen){
      if(canRepair >= weak[weakIdx]) {
        weakIdx++;
      } else {
        break;
      }
    }
    if(weakIdx === weakLen) {
      break;
    }
  } 
  if(weakIdx === weakLen) {
    return count;
  } else {
    return -1;
  }
}

function permutator(inputArr, rValue) {
  var results = [];
  var r = inputArr.length-rValue; // 정확히는 arr의 length가 남는 개수에 따라 result값에 push를 한다. 즉 1개를 썼으면 3개가 남았으니까 1개 result.
  
  function permute(arr, pick) {
  
    var cur, pick = pick || [];

    for (var i = 0; i < arr.length; i++) {
      cur = arr.splice(i, 1);
      if (arr.length === r) {
        results.push(pick.concat(cur));
      }
      permute(arr.slice(), pick.concat(cur));
      arr.splice(i, 0, cur[0]);
    }
    return results;
  }
  return permute(inputArr);
}
```


---
layout: post
author: study
title:  "이분탐색 : 징검다리 건너기"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---
category: 2019 카카오 개발자 겨울 인턴십


# 문제 설명

  [본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

  카카오 초등학교의 니니즈 친구들이 라이언 선생님과 함께 가을 소풍을 가는 중에 징검다리가 있는 개울을 만나서 건너편으로 건너려고 합니다. 라이언 선생님은 니니즈 친구들이 무사히 징검다리를 건널 수 있도록 다음과 같이 규칙을 만들었습니다.

  징검다리는 일렬로 놓여 있고 각 징검다리의 디딤돌에는 모두 숫자가 적혀 있으며 디딤돌의 숫자는 한 번 밟을 때마다 1씩 줄어듭니다.
  디딤돌의 숫자가 0이 되면 더 이상 밟을 수 없으며 이때는 그 다음 디딤돌로 한번에 여러 칸을 건너 뛸 수 있습니다.
  단, 다음으로 밟을 수 있는 디딤돌이 여러 개인 경우 무조건 가장 가까운 디딤돌로만 건너뛸 수 있습니다.
  니니즈 친구들은 개울의 왼쪽에 있으며, 개울의 오른쪽 건너편에 도착해야 징검다리를 건넌 것으로 인정합니다.
  니니즈 친구들은 한 번에 한 명씩 징검다리를 건너야 하며, 한 친구가 징검다리를 모두 건넌 후에 그 다음 친구가 건너기 시작합니다.

  디딤돌에 적힌 숫자가 순서대로 담긴 배열 stones와 한 번에 건너뛸 수 있는 디딤돌의 최대 칸수 k가 매개변수로 주어질 때, 최대 몇 명까지 징검다리를 건널 수 있는지 return 하도록 solution 함수를 완성해주세요.

  - 제한사항
    - 징검다리를 건너야 하는 니니즈 친구들의 수는 무제한 이라고 간주합니다.
    - stones 배열의 크기는 1 이상 200,000 이하입니다.
    - stones 배열 각 원소들의 값은 1 이상 200,000,000 이하인 자연수입니다.
    - k는 1 이상 stones의 길이 이하인 자연수입니다.


# 문제풀이
  문제 사고력이 가장 큰 영향을 주는것 같다.
  stones index의 값이 연속으로 0이 되는 개수가 k값보다 커지기 직전의 건넌 횟수

  핵심 포인트는 결국 stones에 있는 값에서 return 값이 나온다! 라는 것. 
  (stones에 있는 값 보다 1보다 클수도 작을수도 없다.)
  
  처음은 문제 설명을 문자 그대로 이용하여 문제해결하고자 하였다.
  그러다 보니 설계를 다행히 핵심 포인트는 잡았지만
  각 stone별 해당 값을 빼고 값 중 현재값이 양수이면서 0보다 작아지는 index를 바탕으로 k값보다 커지는 순간을 측정하였다.

  정확도도 해결해야 했으나, 속도도 같으 병행하며 문제 해결을 해 나갔는데,

  조금 더 생각해 보면 stones의 값 중 중복된 값을 제외하고 정렬을 한뒤 작은 값부터 빼내면 
  해당 값들이 각 index 별 0이 되는 값이기 때문에 (stone이 사라지는 값) 더 빠르게 사용할수 있었다.

  여기까지가 할 수 있었던 최대한의 해결.


  처음 문제 풀이 : 정확도는 100%, 속도: 0%, 공간: 속도로 인한 측정 불가.
  ```javascript
 function solution(stones, k) {

    let countStone = 0;
    var check;
    const fadeStoneSet = [];
    let countSet = new Set();
    stones.forEach(i => countSet.add(i));
    let stoneSet = [...countSet].sort((a, b) => b - a);
    let curStone = 0;

    curStone = stoneSet.pop();
    if(k===1) {
        return curStone;
    }
    while (curStone !== undefined) {
        let fadeStone = [];
        stones.forEach((item, idx) => {
            if (item === curStone) {
                fadeStone.push(idx);
                countStone++;
            }
        });
        fadeStoneSet.push(...fadeStone);
        fadeStoneSet.sort( (a, b) => a-b);
        let size = fadeStoneSet.length;
        if (countStone >= k) {
            var item = fadeStone.pop();
            while (item !== undefined) {
                check = 1;
                let idx = fadeStoneSet.indexOf(item);
                let start=idx-k;
                if(start<0) { 
                    start=0;
                }
                let end = idx+k;
                if(end>size) {
                    end = size;
                }
                for(let i=start, len=end; i<len; i++){
                    if(check ===2) {
                        debugger;
                    }
                    if((fadeStoneSet[i+1]-fadeStoneSet[i]) === 1) {
                        check++;
                        if(check===k) {
                            return curStone;
                        }
                    } else {
                        check = 1;
                    }
                }
                item = fadeStone.pop();
            }
        }
        curStone = stoneSet.pop();
    }
    return curStone;
}
  ```

 위 같이 해결하고 찾아보니 위의 방법은 사실상 완전탐색과 다름이 없고 
 이분탐색을 한다면 문제 해결은 매우 빨라짐을 알게 되었다.

 그래도 stones 내에 있는 값이 결국은 return 값이 된다는 사실을 통해 
 정렬된 stones 값을 기준으로 left mid right 값을 확인하도록 할 수 있었던 부분에 위안을 삼아야 했다.(다른 풀이를 보니 0과 최대값을 기준으로 이분탐색시도)

  
  최종 풀이 
  
```javascript
function checkStone(stones, mid, k) {
    let now = 0;
    for(let i = 0; i < stones.length; i++) {
        if(stones[i] < mid) { 
            now += 1;
        }
        else {
            now = 0;
        }
        if(now >= k) {
            return false;
        } 
    } 
    return true;
}

function solution(stones, k) {

    let countSet = new Set();
    stones.forEach(i => countSet.add(i));
    let stoneSet = [...countSet].sort((a, b) => a-b);

    let right=stoneSet.length-1;
    let left=0;

    let mid;

    while(left <= right) {
        mid = parseInt((left+right)/2);

        if(checkStone(stones, stoneSet[mid], k)) {
            left = mid+1;
        } else {
            right = mid-1;
        }
    }
    return stoneSet[left-1];
}
```
---
layout: post
author: study
title: "완전탐색 : 괄호 변환"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---
category: 2020 KAKAO BLIND RECRUITMENT


# 문제 설명
 고고학자인 튜브는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

 잠겨있는 자물쇠는 격자 한 칸의 크기가 1 x 1인 N x N 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 M x M 크기인 정사각 격자 형태로 되어 있습니다.

 자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

 열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

 - 제한사항
   - key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
   - lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
   - M은 항상 N 이하입니다.
   - key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
   - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.


 - 입출력 예
 | key | lock | result |
 | [[0, 0, 0], [1, 0, 0], [0, 1, 1]] | [[1, 1, 1], [1, 1, 0], [1, 0, 1]] | true |
 
 
# 문제풀이
  
  문제는 배열을 이용한 작업을 할수 있느냐는 문제이다. 보통 이런 문제는 완전 탐색으로 자주 나오는 것 같다. 배열을 잘 사용할수 있는지 확인하는 차원에서 전체를 검색하도록 말이다.
  고민하기 까지 상당한 시간이 걸렸는데, 자바스크립트는 각 값이 연결되어 있어서, 자바와는 다르게 독립적으로 값을 변경하면서 다룰수가 없었다. 그래서 객체를 이용했다.
  key를 나타내는 객체와 lock에서 비어있는 곳을 나타내는 객체를 만든 뒤, key의 움직임에 따라 lock과 일치할 경우 해결되도록 구현하였다.


```javascript
function solution(key, lock) {
    const keyLen = key.length;
    const lockLen = lock.length;

    let lockPosition = makePosition(lock, 0); //처음엔 lock은 그냥 배열로 해결하려고 했는데, 배열 값을 따로 받아서 바꾸려니 값이 이어져서 바뀌어 버려서 그냥 이것 또한 객체로 만들었다.
    let rotatedKey = rotation(key);
    let rotatedKeyPosition;

    for(let i=0; i<4; i++) { // 마지막으로 헤멘 포인트, 한번 돌려주면 끝나는게 아니라 네번 모두 돌려줘야 한다. 생각해 보면 당연하다.
        rotatedKey = rotation(rotatedKey);
        rotatedKeyPosition = makePosition(rotatedKey, 1);
        
        if(moveMatchKey(rotatedKeyPosition, lockPosition, keyLen, lockLen)) {
            return true;
        }
    }

    return false;
}

function moveMatchKey(keyPosition, lockPosition, keyLen, lockLen) { //position으로 받아오니 key의 size와 lock의 size가 필요해졌다.
    let checkCount = lockPosition.length;
    let count;

    for (let i = -keyLen + 1; i < lockLen; i++) { //움직일수 있는 거리를 설정함. (key size)
        for (let j = -keyLen + 1; j < lockLen; j++) {
            count = 0;
            var caliPositionSet = [];
            keyPosition.forEach(k => {
                var position = {};
                var caliPositionX = k.x + i;
                var caliPositionY = k.y + j;
                if (validation(caliPositionX, lockLen) && validation(caliPositionY, lockLen)) { //lockSize 안에 들어와야 하므로 lockSize가 필요
                    position.x = caliPositionX;
                    position.y = caliPositionY;
                    caliPositionSet.push(position);
                    count++;
                }
            })
            if (checkCount === count) {
                if(checkOkay(caliPositionSet, lockPosition)) {
                    return true;
                }
            }
        }
    }
    return false;
}

function validation(point, pivotSize) {
    if(point<0 || point>=pivotSize){
        return false;
    }
    if(point>=pivotSize){
        return false;
    }
    return true;
} 

function checkOkay(moveKey, lock) {
    var len = lock.length;
    for (let i = 0; i < len; i++) {
        if(moveKey[i].x !== lock[i].x || moveKey[i].y !== lock[i].y) { //완벽히 같아야 인정
            return false;
        } 
    }
    return true;
}

function rotation(key) {
    if(key.length === 1) {
        return key;
    }
    let result = [];
    for(let i=key.length-1; i>=0; i--) {
        result = key[i].map( (_, $) => [...(result[$] || []), key[i][$]]); //잘쓰이는 rotation
    }
    return result; 
}

function makePosition(item, keyOrLock) {
    var positionSet = [];
    if(item.length ===1) {
        var position = {};
        position.x =0;
        position.y =0;
        positionSet.push(position);
        return positionSet;
    }
    item.forEach( (i, rowIdx) => {
        i.forEach( (j, colIdx) => {
            var position = {};
            if(j===keyOrLock) {
                position.x =rowIdx;
                position.y =colIdx;
                positionSet.push(position);
            }
        })
    })

    return positionSet;
}
```


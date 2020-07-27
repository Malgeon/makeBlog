---
layout: post
author: study
title:  "멱집합"
description: "멱집합 구하기"
categories: [ Study ]
tags: [programming, javascript]
---


# 멱집합
  탐색 문제에서 멱집합을 이용해야 하는 경우가 종종 있다.
  
  이해하고 사용하자.

 ## 부분집합
  부분집합을 구성하는 방법은 원소를 사용여부(O/X)로 구분된다.

  이는 이진트리로 구현할수 있는데.
  단계를 구성하고 각 이진트리에 들어갈때마다 flag에 O,X를 기록한다.
  마지막 단계에 들어가게 되면 해당 flag를 바탕으로 해당하는 set을 Array에서 찾고 powerSet에 push한다.

  
```javascript
const getPowerSet = function (arr) {
    let flag = new Array(arr.length).fill(false);
    const powerSet = [];
    let len = arr.length;
    const makePowerSet = (depth) => { 
      if (depth === len) { 
        const subSet = arr.filter((value, idx) => flag[idx]); 
        powerSet.push(subSet); 
        return;
      }
  
      flag[depth] = true; 
      makePowerSet(depth+1);
  
      flag[depth] = false;
      makePowerSet(depth+1);
    }
    
    makePowerSet(0);
    return powerSet;
  }
  
  const example = [1,2,3];
  const result = getPowerSet(example);
  console.log(result);
```
  잘 생각해야 하는건 어차피 각 단계별로 flag를 새로 기입해야 하므로, 함수에 flag를 매개로 넣어줄 필요가 없다.
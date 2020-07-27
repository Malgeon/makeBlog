---
layout: post
author: study
title:  "연습문제 : 스킬트리"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---
category: 

 url: https://programmers.co.kr/learn/courses/30/lessons/49993


# 문제풀이
  주어진 조건에 맞춰 구현을 할수 있는지를 묻는 문제이다.
  문자열을 다루는 문제라고도 할수 있다.

  문제 해결의 방향을 입력에서 오는 문자열을 하나씩 빼면서 검사하도록 잡았는데, 처음 문제를 인식할 때 스킬은 입력한 스킬이며 해당 입력 스킬을 스킬트리 내에서 가능한 개수를 파악하는 것으로 이해했었다. 첫단추부터 잘못 꿰었어서 문제 해결하는 데 시간이 꽤 오래 걸렸다.

  또한, 하나씩 빼면서 하는 검사다 보니 반복조건을 문자열이 비었을 경우로 생각을 해야 했다. 그렇게 되면 마지막에 가지게 되는 문자열은 검사를 해보지 못하게 된다. 그래서 마지막에 추가 검사를 해줌으로 해결하였다. 
  

```javascript
function enableSkill(skill, skill_tree) {
    let compare = skill.slice();
    let forUse = skill_tree.slice();
    
    let tree = forUse[0];
    forUse = forUse.substring(1);
    let target = compare[0];
    compare = compare.substring(1);

    while(forUse != '') {
        if(compare.includes(target)) {
            return false;
        } else if (target === tree) {
            target = forUse[0];
            forUse = forUse.substring(1);
            tree = compare[0]; 
            compare = compare.substring(1);
        } else {
            target = forUse[0];
            forUse = forUse.substring(1);
        }
    }

    if(compare.includes(target)) {
        return false;
    }
    return true;
}
```

 - for문을 사용하여 더 깔끔하게 코드 작성.
```javascript
function solution(skill, skill_trees) {
    var answer = 0;
    skill_trees.forEach( i => {
        if(enableSkill(skill, i)) {
            console.log(i);
            answer++;
        }    
    })
    return answer;
}

function enableSkill(skill, skill_tree) {
    let compare = skill.slice();
    let forUse = skill_tree.slice();
    let tree = compare[0];
    compare = compare.substring(1);

    for(let i=0, len=forUse.length; i<len; i++) {
        if(compare.includes(forUse[i])) {
            return false;
        }
        else if(tree === forUse[i]) {
            tree = compare[0];
            compare = compare.substring(1);
        }
    }

    return true;
}
```
# 다른 풀이
  내가 생각한 방법은, false 조건을 만들어서 구현했다. 그런데 아래 풀이는 true 조건을 만들고 구현한 방법이다.
  

```javascript
function solution(skill, skill_trees) {
    function isCorrect(n) {
        let test = skill.split('');
        for (var i = 0; i < n.length; i++) {
            if (!skill.includes(n[i])) continue;
            if (n[i] === test.shift()) continue;
            return false;
        }
        return true;
    }    
    return skill_trees.filter(isCorrect).length;
}
```

---
layout: post
author: study
title:  "그래프 : 가장 먼 노드"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---


# 문제설명 
  n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

  노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로   주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

  제한사항
    - 노드의 개수 n은 2 이상 20,000 이하입니다.
    - 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
    - vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

| Vertex |	Edge | return |
|---| :---: | ---: |
| 6 |	[[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]] | 3 |

# 풀이
 우선 V,E 를 그래프를 표현한다. 
 행렬과 리스트 둘다 가능하나, 여기선 리스트로 만든다.
 
```javascript
var v = 6;
var edge = [[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]];
const arr = Array(+1).fill(null).map(() => Array());
for(let i of edge) {
        arr[i[0]].push(i[1]);
        arr[i[1]].push(i[0]);
    }
```

 해당 그래프를 BFS 또는 DFS를 통해 순회를 할 수 있는데, 여기선 노드간 거리를 측정하므로 BFS를 이용한다.
 Queue를 이용해 순회를 하며 해당 거리를 추가로 입력해준다.

```javascript
const queue = [[1,0]];  //Node, Distance
const dist = []; //Node 별 Distance 값 저장.

function inputQueue(i, weight) {
  const arrLeng = arr[i].length;

  for(let j=0; j<arrLeng; j++) {
    const num = arr[i].shift();
    queue.push([num, weight]);

    for(let k=1; k<=n; k++) {
      const index = arr[k].indexOf(num);
    if(index != -1)
        arr[k].splice(index, 1);
    }
  }
}
```
그렇게 되면 Max 거리를 알수 있게 되며, max거리를 가진 값의 개수만 return해주면 된다.

```javascript
inputQueue(1, 1);

  while(queue.length > 0) {
    const shifted = queue.shift();

    if(dist[shifted[0]] == null || dist[shifted[0]]>shifted[1])
      dist[shifted[0]] = shifted[1];

    inputQueue(shifted[0], dist[shifted[0]]+1);
  }

  let max = 0;
  let answer = 0;

  for(let i=1; i<=n; i++) {
      if(dist[i]>max) {
          max = dist[i];
          answer = 1;
      } else if(dist[i] == max) {
          answer++;
      }
  }
    return answer;
```

 해결 Point: 그래프 표현으로 전환 -> BFS 순회하며 거리측정 -> Max 거리 count
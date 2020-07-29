---
layout: post
author: test
title: "BFS : 블록 이동하기"
description: "2020 KAKAO BLIND RECRUITMENT"
categories: [ test ]
tags: [standard ]
testFeatured: true
hidden: true
---

# 문제 설명
 로봇개발자 무지는 한 달 앞으로 다가온 카카오배 로봇경진대회에 출품할 로봇을 준비하고 있습니다. 준비 중인 로봇은 2 x 1 크기의 로봇으로 무지는 0과 1로 이루어진 N x N 크기의 지도에서 2 x 1 크기인 로봇을 움직여 (N, N) 위치까지 이동 할 수 있도록 프로그래밍을 하려고 합니다. 로봇이 이동하는 지도는 가장 왼쪽, 상단의 좌표를 (1, 1)로 하며 지도 내에 표시된 숫자 0은 빈칸을 1은 벽을 나타냅니다. 로봇은 벽이 있는 칸 또는 지도 밖으로는 이동할 수 없습니다. 로봇은 처음에 아래 그림과 같이 좌표 (1, 1) 위치에서 가로방향으로 놓여있는 상태로 시작하며, 앞뒤 구분없이 움직일 수 있습니다.

 로봇이 움직일 때는 현재 놓여있는 상태를 유지하면서 이동합니다. 예를 들어, 오른쪽으로 한 칸 이동한다면 (1, 2), (1, 3) 두 칸을 차지하게 되며, 아래로 이동한다면 (2, 1), (2, 2) 두 칸을 차지하게 됩니다. 로봇이 차지하는 두 칸 중 어느 한 칸이라도 (N, N) 위치에 도착하면 됩니다.

 로봇은 다음과 같이 조건에 따라 회전이 가능합니다.

 로봇은 90도씩 회전할 수 있습니다. 단, 로봇이 차지하는 두 칸 중, 어느 칸이든 축이 될 수 있지만, 회전하는 방향(축이 되는 칸으로부터 대각선 방향에 있는 칸)에는 벽이 없어야 합니다. 로봇이 한 칸 이동하거나 90도 회전하는 데는 걸리는 시간은 정확히 1초 입니다.

 0과 1로 이루어진 지도인 board가 주어질 때, 로봇이 (N, N) 위치까지 이동하는데 필요한 최소 시간을 return 하도록 solution 함수를 완성해주세요.

 - 제한사항
   - board의 한 변의 길이는 5 이상 100 이하입니다.
   - board의 원소는 0 또는 1입니다.
   - 로봇이 처음에 놓여 있는 칸 (1, 1), (1, 2)는 항상 0으로 주어집니다.
   - 로봇이 항상 목적지에 도착할 수 있는 경우만 입력으로 주어집니다.

 - 입출력 예

 | board | result |
 | [[0, 0, 0, 1, 1],[0, 0, 0, 1, 0],[0, 1, 0, 1, 1],[1, 1, 0, 0, 1],[0, 0, 0, 0, 0]] | 7 |

# 문제풀이
  어떤방식으로 설계를 해야할지 부터가 고민이었다.
  상하 좌우 이동은 단순히 해당 point로부터 각각 옮기면 되었지만, 2개의 축을 두고 회전하는 것을 표현하는 방법을 설계하기가 어려웠다.

  해결책은 생각보다 간단한데, 그냥 기준점을 하나의 point로 가지고 진행을 하면 된다.
  가로상태일때 기준점은 좌측, 세로일때 기준점을 위측 으로 잡는다.
  그렇게 되면 상하좌우는 단순히 기준점만 옮기면 되며, 2개의 축을 가지고 회전하는 경우 각각의 상태를 그냥 표현해 주면 된다.
  좌측기준 위로 옮기기 => 기준점을 상향, 우측 기준 위로 옮기기 => 기준점을 우측 이동 후 상향
  또한 rotate 조건은 기준점을 상하좌우로 옮기는 조건과 동일하다. 

  이 기준을 가지고 해결해 주면 된다.
  
  위와 같이 기준점이 바뀌는 복잡한 경우 그냥 하나의 기준점을 잡아주고 그 기준을 바탕으로 조건을 설정해주면 해결할 수 있다.
  
  또한 가장 빠른 시간(가장 낮은 걸음수)에 도착하는 문제 같은 경우 해당 point에 시간(걸음수)를 간직한채 방문을 해주면, 해당 값을 update하면서 풀어 주면 된다.

  
```javascript

function solution(map) {
  const n = map.length,
    dx = [0, 0, 1, -1], //direction x, y 상 하 좌 우 를 나타낸다.
    dy = [1, -1, 0, 0],
    rowToColDx = [-1, -1, 0, 0], //가로에서 세로로 변경할 좌표. 좌측 up, 우측 up, 좌측 down, 우측 down.
    rowToColDy = [0, 1, 0, 1],
    colToRowDx = [0, 1, 0, 1],  // 세로에서 가로로 변경할 좌표. 위와 동일.
    colToRowDy = [0, 0, -1, -1],
    q = [
      [0, 0, 0, 0] //starting point, BFS의 Queue를 받는다. //이렇게 BFS를 사용할 수 있다.
    ],

    visited = []; // 방문할때마다, 방문자의 시간을 보고 가장 낮은 값을 간직.


  for (let i = 0; i < n; i++) {
    visited.push([]);
    for (let j = 0; j < n; j++) {
      visited[i].push([Infinity, Infinity]); //가장 낮은값을 간직하기 위해 Infinity 설정
    }
  }

  visited[0][0][0] = 0; //처음은 0초

  let min = Infinity;

  const canGoRight = (x, y, p) => { //x,y는 좌표 p는 상태 (가로 상태, 세로 상태)
    if (p === 0) {
      if (y + 2 >= n || map[x][y + 1] === 1 || map[x][y + 2] === 1)
        return false;
    } else {
      if (y + 1 >= n || x + 1 >= n || map[x][y + 1] === 1 || map[x + 1][y + 1] === 1)
        return false;
    }
    return true;
  };

  const canGoLeft = (x, y, p) => {
    if (p === 0) {
      if (y - 1 < 0 || map[x][y - 1] === 1)
        return false;
    } else {
      if (y - 1 < 0 || x + 1 >= n || map[x][y - 1] === 1 || map[x + 1][y - 1] === 1)
        return false;
    }
    return true;
  };

  const canGoDown = (x, y, p) => {
    if (p === 0) {
      if (x + 1 >= n || y + 1 >= n || map[x + 1][y] === 1 || map[x + 1][y + 1] === 1)
        return false;
    } else {
      if (x + 2 >= n || map[x + 1][y] === 1 || map[x + 2][y] === 1) {
        return false;
      }
    }
    return true;
  };

  const canGoUp = (x, y, p) => {
    if (p === 0) {
      if (x - 1 < 0 || y + 1 >= n || map[x - 1][y] === 1 || map[x - 1][y + 1] === 1)
        return false;
    } else {
      if (x - 1 < 0 || map[x - 1][y] === 1) {
        return false;
      }
    }
    return true;
  };

  const canRotateUp = (x, y) =>
    !(x - 1 < 0 || y + 1 >= n || map[x - 1][y] === 1 || map[x - 1][y + 1] === 1);

  const canRotateDown = (x, y) =>
    !(x + 1 >= n || y + 1 >= n || map[x + 1][y] === 1 || map[x + 1][y + 1] === 1);

  const canRotateRight = (x, y) =>
    !(y + 1 >= n || x + 1 >= n || map[x][y + 1] === 1 || map[x + 1][y + 1] === 1);

  const canRotateLeft = (x, y) =>
    !(y - 1 < 0 || x + 1 >= n || map[x][y - 1] === 1 || map[x + 1][y - 1] === 1);

  const canGoMap = [ //한번에 이동 및 rotate를 해주기 위한 작업. canGoMap[0] => canGoRight
    canGoRight,
    canGoLeft,
    canGoDown,
    canGoUp
  ],
    canRotateMap = [
      [
        canRotateUp,
        canRotateUp,
        canRotateDown,
        canRotateDown
      ], [
        canRotateRight,
        canRotateRight,
        canRotateLeft,
        canRotateLeft
      ],
    ],

    deltaMap = [
      [rowToColDx, rowToColDy],
      [colToRowDx, colToRowDy]
    ];



  while (q.length) {
    debugger;
    const [x, y, t, p] = q.shift();

    // 만일 (n,n)에 도착했다면 최소시간을 갱신합니다
    if (
      (x === n - 1 && y === n - 2 && p === 0)
      || (x === n - 2 && y === n - 1 && p === 1)
    ) {
      debugger;
      min = Math.min(min, t);
    }

    // 회전 없이 우좌하상으로 움직일때
    for (let k = 0; k < 4; k++) {
      if (!canGoMap[k](x, y, p)) {
        continue;
      }

      const nx = x + dx[k],
        ny = y + dy[k];

      if (visited[nx][ny][p] > t + 1) {
        visited[nx][ny][p] = t + 1;
        q.push([nx, ny, t + 1, p]);
      }
    }

    for (let k = 0; k < 4; k++) {
      // 가로로 놓인 상황인 경우 세로로 돌려보자
      // 세로로 놓인 상황인 경우 가로로 돌려보자

      if (!canRotateMap[p][k](x, y)) continue;

      const nx = x + deltaMap[p][0][k],
        ny = y + deltaMap[p][1][k],
        _p = p ? 0 : 1;

      if (visited[nx][ny][_p] > t + 1) {
        visited[nx][ny][_p] = t + 1;
        q.push([nx, ny, t + 1, _p]);
      }
    }

  }

  return min;
}
```


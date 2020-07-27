---
layout: post
author: study
title:  "스택 : 크레인 인형뽑기 게임"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---
category: 2019 카카오 개발자 겨울 인턴십


# 문제 설명

  게임개발자인 죠르디는 크레인 인형뽑기 기계를 모바일 게임으로 만들려고 합니다.
  죠르디는 게임의 재미를 높이기 위해 화면 구성과 규칙을 다음과 같이 게임 로직에 반영하려고 합니다.

  게임 화면은 1 x 1 크기의 칸들로 이루어진 N x N 크기의 정사각 격자이며 위쪽에는 크레인이 있고 오른쪽에는 바구니가 있습니다. (위 그림은 5 x 5 크기의 예시입니다). 각 격자 칸에는 다양한 인형이 들어 있으며 인형이 없는 칸은 빈칸입니다. 모든 인형은 1 x 1 크기의 격자 한 칸을 차지하며 격자의 가장 아래 칸부터 차곡차곡 쌓여 있습니다. 게임 사용자는 크레인을 좌우로 움직여서 멈춘 위치에서 가장 위에 있는 인형을 집어 올릴 수 있습니다. 집어 올린 인형은 바구니에 쌓이게 되는 데, 이때 바구니의 가장 아래 칸부터 인형이 순서대로 쌓이게 됩니다.

  만약 같은 모양의 인형 두 개가 바구니에 연속해서 쌓이게 되면 두 인형은 터뜨려지면서 바구니에서 사라지게 됩니다. 

  크레인 작동 시 인형이 집어지지 않는 경우는 없으나 만약 인형이 없는 곳에서 크레인을 작동시키는 경우에는 아무런 일도 일어나지 않습니다. 또한 !!바구니는 모든 인형이 들어갈 수 있을 만큼 충분히 크다고 가정합니다. 

  게임 화면의 격자의 상태가 담긴 2차원 배열 board와 인형을 집기 위해 크레인을 작동시킨 위치가 담긴 배열 moves가 매개변수로 주어질 때, 크레인을 모두 작동시킨 후 터트려져 사라진 인형의 개수를 return 하도록 solution 함수를 완성해주세요.

 - 제한사항
   - board 배열은 2차원 배열로 크기는 5 x 5 이상 30 x 30 이하입니다.
   - board의 각 칸에는 0 이상 100 이하인 정수가 담겨있습니다.
   - 0은 빈 칸을 나타냅니다.
   - 1 ~ 100의 각 숫자는 각기 다른 인형의 모양을 의미하며 같은 숫자는 같은 모양의 인형을 나타냅니다.
   - moves 배열의 크기는 1 이상 1,000 이하입니다.
   - moves 배열 각 원소들의 값은 1 이상이며 board 배열의 가로 크기 이하인 자연수입니다.

# 문제풀이
  단순 스택문제 풀이이다.

  이걸 자바로 풀 생각해보니 스택 imort하고, 선언하고.. 어휴 상상만으로 머리가 아프다. 이래서 파이썬 자바스크립트 하는듯 하다.
  자바스크립트는 push & pop을 기본 지원하지만 peek은 지원하지 않아서 Stack을 함수로 만들어줘야하는것 같다.

```javascript
function solution(board, moves) {
    var answer = 0;
    const stack = new Stack();
    moves.forEach( idx => {
        var target = board.find(_ => _[idx-1]);
        if (!(target === undefined)) {
            var moveItem = target.splice(idx-1, 1, 0);
            if (stack.peek() === moveItem[0]) {
                stack.pop();
                answer++;
            } else {
                stack.push(moveItem[0]);
            }
        }   
    })
    return answer*2;
}

class Stack {
    constructor() {
        this._arr = [];
    }

    push(item) {
        this._arr.push(item);
    }

    pop() {
        return this._arr.pop();
    }

    peek() {
        return this._arr[this._arr.length -1];
    }
}
```
# 더 좋은 풀이
  역시 단순한 문제여도 좋은 테크닉은 계속 나오는 것 같다... (물론 아주 작은 차이라고 생각은 되지만 그 차이가 쌓이면 큰 차이가 되겠지.) 앞서 한 말 취소..
  기존 문제 풀이 같은 경우. moves에서 나온 idx를 기준으로 row별 Serch하여 0이 아닌값을 찾으며 해당 값을 없애는 방식으로 사용했다.

  그러면 undefined이 나올 때까지 매번 find를 해야 하는 반복 이슈를 해결하기 위해 0이 아닌값을 찾아 아예 스택(peek이 필요없는)에 쌓아둔다.
  그러면 moves에서 나온 idx에 해당하는 stack row를 pop만 해주면 되는 것 아닌가!

   테크닉 : matric의 row를 colunm으로 전체 전환-> 전환 row의 0값을 없애주고 순서 보정(반대 정렬) -> 사용

```javascript
const transpose = (matrix) =>
  matrix.reduce(
  (result, row) => row.map((_, i) => [...(result[i] || []), row[i]]),
  []
);

function solution(board, moves) {
    var answer = 0;
    const boardStacks = transpose(board).map(row =>
        row.reverse().filter(el => el !== 0)
    );
    debugger;
    const stack = new Stack();

    moves.forEach( idx => {
        var target = boardStacks[idx-1].pop();
        if (target !== undefined) {
            if (stack.peek() === target) {
                stack.pop();
                answer++;
            } else {
                stack.push(target);
            }
        }   
    })
    return answer*2;
}

class Stack {
  constructor() {
    this._arr = [];
  }

  push(item) {
    this._arr.push(item);
  }

  pop() {
    return this._arr.pop();
  }

  peek() {
    return this._arr[this._arr.length -1];
  }
}

```
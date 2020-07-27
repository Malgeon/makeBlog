---
layout: post
author: study
title:  "Graph [BFS, DFS]"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
---


# Graph [DFS, BFS]
  자바 스크립트를 이용한 개념 구현
   
* * *

 ## Graph

  ![SAMPLE]({{ site.asset_url }}/images/javascript/sample/dfs.png "SAMPLE")

  위와 같은 Graph를 구현해 보자.

  | Vertex |	Edge | 
  |---| :---: | 
  | 8 |	[[1, 2], [1, 3], [2, 4], [2, 5], [3, 6], [3, 7], [4, 8], [5, 8], [6, 8], [7, 8]] |

  - Vertex는 1부터 n까지 이며, Weight가 없는 경우 

  ```javascript
  var edge = [[1, 2], [1, 3], [2, 4], [2, 5], [3, 6], [3, 7], [4, 8], [5, 8], [6, 8], [7, 8]];
  var v = 8;
  const arr = Array(v+1).fill(null).map(() => Array());
  for(let i of edge) {
    arr[i[0]].push(i[1]);
    arr[i[1]].push(i[0]);
  }
  ```

 - Vertex, Edge, Weight 전체 관리 

 ```javascript
class UndirectGraph {
  edge = {};
  addVertex(vertex) {
    this.edge[vertex] = {};
  }
  addEdge(vertex1, vertex2, weight) {
    this.edge[vertex1][vertex2] = weight;
    this.edge[vertex2][vertex1] = weight;
  }
}

const graph = new UndirectGraph();
graph.addVertex(1);
graph.addVertex(2);
graph.addEdge(1, 2, 5);
```

그래프를 구현하였으니 이제 DFS와 BFS를 이용해 탐색을 해보자.

 ## DFS
  - 노드를 설정한 방향대로 끝까지 간 뒤 가장 최근으로 backtacking을 진행하여 해당 방향 끝까지 보내며 전체 탐색을 하는 방법
  - 재귀 호출 방법과 스택을 사용한 방법이 있다.

 `1→2→4→8→5→(8)→6→3→(6)→(8)→7→(8)→(4)→(2)→(1)`

  - 재귀

```javascript
const graph = (arr, start) => {
    let visited = new Array(arr.length).fill(false);
    const recDFS = (idx) => { 
        visited[idx] = true;
        console.log(idx + " ");
        arr[idx].forEach( i => {
            if(visited[i] == false) {
                recDFS(i);
            }
        })
        
        return;
    }
    return recDFS(start); // idx start 부터 시작
}

graph(arr,1);
```

- 스택

```javascript
const graph = (arr, start) => {
    
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
          return this._arr[this._arr.length - 1];
        }
    }
    const stack = new Stack();
    let visited = new Array(arr.length).fill(false);
    const recDFS = (idx) => { 
        stack.push(idx);
        let flag;
        visited[idx] = true;
        console.log(idx);

        let value = stack.peek();
        while(!(value === undefined)) {
            flag = false;
            for( i of arr[value]) {
                if(visited[i] === false) {
                    stack.push(i);
                    console.log(i);
                    visited[i] = true;
                    flag = true;
                    break;
                }
            }
            if(!flag) {
                stack.pop();
            }
            value = stack.peek();
        }
        return;
    }
    return recDFS(start); // idx start 부터 시작
}

```


## BFS

 - 한 노드에 연결된 노드를 전체 탐색한 뒤 설정한 방향대로 차례로 진행하며 전체를 탐색 하는 방법
 - 큐를 사용한다.

 `1→(1)→2→3→(2)→4→5→(3)→6→7→(4)→8→(5)→(6)→(7)→(8)`


```javascript
const graph = (arr, start) => {
    
    class Queue {
        constructor() {
          this._arr = [];
        }
        enqueue(item) {
          this._arr.push(item);
        }
        dequeue() {
          return this._arr.shift();
        }
        peek() {
            return this._arr[0];
        }
    }

    const queue = new Queue();
    let visited = new Array(arr.length).fill(false);
    const recBFS = (idx) => { 
        queue.enqueue(idx);
        let flag;
        visited[idx] = true;
        console.log(idx);
        let value = queue.peek();
        while(!(value === undefined)) {
            flag = false;
            for( i of arr[value]) {
                if(visited[i] === false) {
                    queue.enqueue(i);
                    console.log(i);
                    visited[i] = true;
                    flag = true;
                }
            }
            if(!flag) {
                queue.dequeue();
            }
            value = queue.peek();
        }
        return;
    }
    return recBFS(start); // idx start 부터 시작
}
```

 DFS, BFS의 컨셉을 이용해 문제를 해결해야 하는 것들이 있음을 인지하자.

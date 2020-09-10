---
layout: post
author: study
title:  "Graph"
description: "무방향 그래프, 방향 그리고 가중치 그래프"
categories: [ study ]
tags: [programming, kotlin]
postImgOn: false
image: assets/images/study/kotlin.png
---

### Graph
그래프를 다뤄보자.

- 정점 (Vertex / Node) <br>
    그래프에서 위치를 나타냅니다.
- 간선 (Edge / Link / Branch) <br>
    그래프에서 위치간의 관계를 나타냅니다. 즉, 각 정점(노드)를 연결하는 선을 의미합니다.
- 인접 정점 (Adjacent Vertex) <br>
    간선(Edge)에 의해 직접 연결된 정점을 의미합니다.
- G(V, E) <br>
    그래프는 정점과 간선의 집합이므로 G(V, E)는 그래프 자체를 의미합니다.



#### 무방향 그래프(undirected graph)

<div class="card h-100 my-u-padding"><div class="insertcover"><div class=""><img class="inserturl" src="{{ site.baseurl }}/assets/images/study/Graph.png" alt="programmers.co.kr"/></div></div></div>

| Vertex |	Edge | 
| :---: | :---: | 
| 8 | [[1, 2], [1, 3], [2, 4], [2, 5], [3, 6], [3, 7], [4, 8], [5, 8], [6, 8], [7, 8]] |

```java
val edge = arrayOf(intArrayOf(1, 2), intArrayOf(1, 3), intArrayOf(2, 4),
            intArrayOf(2, 5), intArrayOf(3, 6), intArrayOf(3, 7),
            intArrayOf(4, 8), intArrayOf(5, 8), intArrayOf(6, 8), 
            intArrayOf(7, 8))
```

- 행렬

```java
val arr = Array(9) { Array(9) { 0 } }
// 무뱡향
edge.forEach {
        arr[it[0]][it[1]] = 1 
        arr[it[1]][it[0]] = 1
    }
arr.forEach {
        println(Arrays.toString(it))
    }
```
```
[0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 1, 1, 0, 0, 0, 0, 0]
[0, 1, 0, 0, 1, 1, 0, 0, 0]
[0, 1, 0, 0, 0, 0, 1, 1, 0]
[0, 0, 1, 0, 0, 0, 0, 0, 1]
[0, 0, 1, 0, 0, 0, 0, 0, 1]
[0, 0, 0, 1, 0, 0, 0, 0, 1]
[0, 0, 0, 1, 0, 0, 0, 0, 1]
[0, 0, 0, 0, 1, 1, 1, 1, 0]
```

- 리스트

vertex 할당

```java
val list = List(9) { ArrayList<Int>() }
// 무뱡향
edge.forEach {
        list[it[0]].add(it[1])
        list[it[1]].add(it[0])
    }
list.forEach {
        println(it.toString())
    }
```
```
[]
[2, 3]
[1, 4, 5]
[1, 6, 7]
[2, 8]
[2, 8]
[3, 8]
[3, 8]
[4, 5, 6, 7]
```

#### 방향 그래프(digraph, directed graph), 가중치 그래프(weighted graph)

<div class="card h-100 my-u-padding"><div class="insertcover"><div class=""><img class="inserturl" src="{{ site.baseurl }}/assets/images/study/Weighted_Direction_Graph.png" alt="programmers.co.kr"/></div></div></div>


| Vertex |	Edge [vertex1, vertex2, weight] | 
| :---: | :---: | 
| 5 | [[1, 2, 12], [1, 3, 6], [1, 4, 10], [2, 3, 10], [2, 5, 2], [3, 4, 3], [4, 2, 2], [4, 5, 5]] |

```java
val edge = arrayOf(intArrayOf(1, 2, 12), intArrayOf(1, 3, 6), 
            intArrayOf(1, 4, 10), intArrayOf(2, 3, 10), intArrayOf(2, 5, 2),
            intArrayOf(3, 4, 3), intArrayOf(4, 2, 2), intArrayOf(4, 5, 5))
```

- 행렬

```java
val infinite = 99999999
val arr = Array(6) { Array(6) { infinite } } 
// 1 ~ 5 의 vertex를 가진다. weight 초기값은 무한
// 단방향
edge.forEach {
        arr[it[0]][it[1]] = it[3]
    }
arr.forEach {
        println(Arrays.toString(it))
    }
```
```
[∞, ∞,  ∞,  ∞,  ∞,  ∞]
[∞, ∞, 12,  6, 10,  ∞]
[∞, ∞,  ∞, 10,  ∞,  2]
[∞, ∞,  ∞,  ∞,  3,  ∞]
[∞, ∞,  2,  ∞,  ∞,  5]
[∞, ∞,  ∞,  ∞,  ∞,  ∞]
```

- 리스트

```java
val list = List(6) { ArrayList<Pair<Int, Int>>() } // 1 ~ 5 의 vertex를 가진다.
// vertex의 개수는 추가해주는 것보다 초기부터 설정해놓고 
// 안쓰는 것은 빈공간으로 두는 것도 하나의 방법이다
// 단방향
edge.forEach {
        list[it[0]].add(Pair(it[1], it[2]))
        // Pair(edge, weigth)
    }
    
list.forEach {
        println(it.toString())
    }
```
```
[]
[(2, 12), (3, 6), (4, 10)]
[(3, 10), (5, 2)]
[(4, 3)]
[(2, 2), (5, 5)]
[]
```


### vertex가 좌표이며 동적으로 주어지는 경우



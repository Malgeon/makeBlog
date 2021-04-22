---
layout: post
author: test
title:  "2021 KAKAO BLIND RECRUITMENT : 합승 택시 요금"
description: "그래프, Dijkstra, Floyd-Warshal"
categories: [ test ]
tags: [ graph ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/72413"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2021 KAKAO BLIND RECRUITMENT : 합승 택시 요금</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

Dijkstra 또는 Floyd-Warshal 알고리즘을 알고 있다면 금방 풀수 있는 문제.

정답률 : 정확성 9.60%, 효율성 7.45%

그럼에도 정답률이 낮은 요인은 3가지로 생각된다.

1. 앞선 3 문제를 풀고난 후 주어진 적은 시간
2. 위와 같은 알고리즘을 모르거나 이해하는 수준의 지식
3. Dijkstra를 list로 풀 경우 정확성만 답을 맞출수 있게 된다. (효율성이 조금 낮은 요인)

우선 Dijkstra로 풀어보았고 나중에 Floyd-Warsha를 업데이트 해보려 한다.


```java
class Solution {
    fun solution(n: Int, s: Int, a: Int, b: Int, fares: Array<IntArray>) 
    = DijkstraClass(n, fares).solve(s, a, b)
}
class DijkstraClass constructor(private val n: Int, private val fares: Array<IntArray>) {
    private var graph = Array(n + 1) { IntArray(n + 1) { 1000000 } }
    private fun makeGraph() {
        graph.forEachIndexed { i, v -> v[i] = 0 }

        for (fare in fares) {
            graph[fare[0]][fare[1]] = fare[2]
            graph[fare[1]][fare[0]] = fare[2]
        }
    }

    init {
        makeGraph()
    }

    private fun dijkstra(start: Int) {
        var minDistance: Int
        var cur = start
        var visited = MutableList(graph.size) { 0 }

        visited[0] = 1
        visited[start] = 1

        while (visited.any { it == 0 }) {
            minDistance = Int.MAX_VALUE
            for (i in 1 until graph.size) {
                if (visited[i] == 0) {
                    if (minDistance > graph[start][i]) {
                        minDistance = graph[start][i]
                        cur = i
                    }
                }
            }
            visited[cur] = 1
            for (i in 1 until graph.size) {
                for (j in 1 until graph.size) {
                    if (graph[i][j] > graph[i][cur] + graph[cur][j]) {
                        graph[i][j] = graph[i][cur] + graph[cur][j]
                    }
                }
            }
        }
    }

    fun solve(s:Int, a: Int, b:Int): Int {
        var minDistance = Int.MAX_VALUE
        dijkstra(b)
        for (i in 1..n) {
            var distance = graph[s][i] + graph[i][a] + graph[i][b]
            if (minDistance > distance) minDistance = distance
        }
        return minDistance
    }
}
```
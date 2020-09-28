---
layout: post
author: test
title:  "DFS/BFS : 타겟 넘버"
description: "연습문제"
categories: [ test ]
tags: [ dfs ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/43165"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">DFS/BFS : 타겟 넘버</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


DFS라기 보다는 재귀를 이용한 완전탐색이라고 할수 있겠다.
그렇다는 말은 반복문으로도 풀수 있다는 말이다.

- 재귀를 이용한 풀이

```java
class Solution {
    fun solution(numbers: IntArray, target: Int): Int {
        return findNum(numbers, target)
    }

    fun findNum(numbers: IntArray, target: Int):Int {
        var num = 0
        fun dfs(numbers: IntArray, target: Int, value: Int, idx: Int) {
            val len = numbers.size
            if (len == idx) {
                if (value == target) {
                    num++
                }
                return
            }
            dfs(numbers, target, value + numbers[idx], idx+1)
            dfs(numbers, target, value - numbers[idx], idx+1)
        }
        dfs(numbers, target, 0, 0)
        return num
    }
}
```


- 반복문을 이용한 풀이

```java
class Solution {
    fun solution(numbers: IntArray, target: Int): Int {
    return numbers.fold(listOf(0)) { list, i ->
            list.run {
                map { it + i } + map { it - i }
            }
        }.count { it == target }
    }
}
```

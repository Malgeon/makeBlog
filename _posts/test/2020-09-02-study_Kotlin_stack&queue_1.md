---
layout: post
author: test
title:  "스택/큐 : 프린터"
description: "우선순위 큐"
categories: [ test ]
tags: [ stack, queue ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42587"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">스택/큐 : 프린터</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

생각하는데 조금 애를 먹었다.

문제의 해결과정은 다음과 같다.

대기 목록에서 꺼낸 문서의 중요도 보다 대기 목록의 중요도가 높을 경우 꺼낸 문서를 대기 문서 맨 뒷부분으로 넣는다.

- 직접 대기 목록을 꺼내 맨 뒷부분으로 넣으려고 했으나, 이 같은 과정은 시간 소요가 너무 크다

- 그렇다면 다른 방법을 생각을 해야 하는 것인데, 대기목록 순서대로 인쇄를 하나 중요도가 있을 시 맨 뒤로 옮겨지는 것을 다시 생각해 보면 대기 목록 순서대로 꺼내는 동작을 반복적으로 하면서 중요도와 맞으면 꺼내고 아니면 지나간다. 이때, 해당 동작하나마다 count를 세는 병렬적인 방식으로 문제 해결방향을 잡았다. 

이때 중요도를 꺼내는 방법으로 우선순위 큐를 사용하였다.

```java
class Solution {
    fun solution(priorities: IntArray, location: Int): Int {
        var answer = 1
        val queue = PriorityQueue(Comparator.reverseOrder<Int>())
        for (i in priorities) {
            queue.offer(i)
        }
        while (!queue.isEmpty()) {
            for (i in priorities.indices) { 
                if (priorities[i] == queue.peek()) {
                    if (location == i) {
                        return answer
                    }
                    answer++
                    queue.poll()
                }
            }
        }
        return answer
    }
}
```
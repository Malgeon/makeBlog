---
layout: post
author: test
title:  "해시 : 위장"
description: "연습 문제"
categories: [ test ]
tags: [ hash ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42578"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">해시 : 위장</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


독립된 2개의 집합간 조합의 수를 나타낸다.
이 때, 해다 집합의 경우에서 하나를 추가해줘야 하는데 그것은 바로 안 입는 경우이다.
또한 모두 안 입는 경우는 제외하므로 결과값에서 1을 빼줘야 한다.

따라서 경우의 수를 단순히 숫자로서 계산을 하면 (집합의 원소 개수 + 1) * (집합의 원소 개수 + 1) - 1 이 된다.

```java
class Solution {
    fun solution(clothes: Array<Array<String>>): Int {
        var clothesMap = mutableMapOf<String, Int>()
        clothes.forEach {
            if(!clothesMap.contains(it[1])) clothesMap[it[1]] = 1
            else {
                clothesMap[it[1]]?.let { value ->
                    clothesMap[it[1]] = value + 1 
                    // 굳이 이것을 하지 않고 size로 확인이 가능하다.
                }
            }
        }
        var answer = 0
        answer = clothesMap.values.fold(1) { total, next ->
            total * (next + 1)
        }
        return answer-1
    }
}
```

코틀린은 groupBy를 지원하는데 이를 사용하면 위의 풀이를 단 한줄에 해결할 수 있다.

```java
lass Solution {
    fun solution(clothes: Array<Array<String>>): Int {
        return clothes.groupBy { it[1] }.values.fold(1) { acc, v -> acc * (v.size + 1) }  - 1
    }
}
```
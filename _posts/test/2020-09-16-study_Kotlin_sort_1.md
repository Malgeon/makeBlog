---
layout: post
author: test
title:  "정렬 : 가장 큰 수"
description: "연습 문제"
categories: [ test ]
tags: [ greedy ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42746"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">정렬 : 가장 큰 수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

문제의 컨셉을 잘 이해하고 해결 방향을 잡아야 문제 풀이가 쉬워진다.
처음 문제 해결 방향을 자리수가 달라질 때, 큰 값을 구하려고 생각을 한다면 문제가 복잡해 진다.
이럴땐 단순히 내림차순으로 정렬을 하는데, 해당 정렬의 기준을 어떻게 하면 문제에 맞게 될 것인지를 생각하도록 해야한다.

문제 해결을 위한 사고력이 가장 중요했던 문제.

그런데 문제 풀이 자체는 쉬웠지만 런타임 오류를 해결하지 못해서 시간이 많이 걸렸다.

오류는 Comparator의 동작원리를 숙지하지 못해서 일어났다.

Comparator 인터페이스에 존재하는 compare를 kotlin은 아래와 같이 설명한다.

```
abstract fun compare(a: T, b: T): Int
```
Compares its two arguments for order. Returns zero if the arguments are equal, a negative number if the first argument is less than the second, or a positive number if the first argument is greater than the second.

같으면 0을 return 한다.

단순히 0 또는 -1이면 sort가 되지 않으므로 같아도 -1로 인식하도록 했더니, 런타임 오류가 발생하였다.
이를 방지하기 위해 0을 입력을 해주던가, 사실 그냥 compareTo를 사용하면 한줄에 해결이 가능하다.


- 런타임 오류 풀이

```java
class Solution {
    fun solution(numbers: IntArray): String {
        var answer = ""
        numbers.sortedWith(Comparator { a: Int, b: Int ->
            when {
                "$b$a" > "$a$b" -> 1
                else -> -1
            }
        }).forEach{ answer += it }
        return if(answer[0] == '0') "0" else answer
    }
}
```


- 문제 풀이

```java
class Solution {
    fun solution(numbers: IntArray): String {
        var answer = ""
        numbers.sortedWith(Comparator { a: Int, b: Int ->
            when {
                "$b$a" > "$a$b" -> 1
                "$b$a" == "$a$b" -> 0
                else -> -1
            }
        }).forEach{ answer += it }
        return if(answer[0] == '0') "0" else answer
    }
}
```

- compareTo 이용ㄴ

```java
class Solution {
    fun solution(numbers: IntArray): String {
        var answer = ""
        numbers.sortedWith(Comparator { a: Int, b: Int ->
            "$b$a".compareTo("$a$b")
        }).forEach{ answer += it }
        return if(answer[0] == '0') "0" else answer
    }
}
```
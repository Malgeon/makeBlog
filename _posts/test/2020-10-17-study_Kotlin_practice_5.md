---
layout: post
author: test
title:  "연습문제 : JadenCase 문자열 만들기"
description: "문자열 다루기"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12951"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : JadenCase 문자열 만들기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

단순하게 주어진 s를 공백으로 나눈 다음 joinToString으로 다시 합치면서 각각의 문자에 대하여 람다식을 적용하는데, 첫글자를 숫자가 아니면 대문자를 하도록 적용하는 방식으로 문제 해결을 하였다.
그런데 s는 알파벳과 공백문자(" ")로 이루어져 있기 때문에, 만약 s에서 "a "로 주어질 경우 까지 생각해서 문제해결을 해야 했는데...

```java
class Solution {
    fun solution(s: String) = s.split(" ").joinToString(" ") {
        var rtn = ""
        if(it != "") {
            val first = it.substring(0..0)
            first.toIntOrNull()?.let { number ->
                rtn += number
            } ?: run {
                rtn += first.toUpperCase()
            }
            rtn += it.substring(1 until it.length).toLowerCase()
        }
        rtn
    }
}
```

코틀린에서는 capitalize를 지원한다.

```java
class Solution {
    fun solution(s: String) = s.toLowerCase().split(" ").joinToString(" ") {
        it.capitalize()
    }
}
```

참고로, joinToString은 map의 기능도 같이 할수 있는데 위의 답안을 map을 이용할 경우 아래와 같다

```java
class Solution {
    fun solution(s: String) = s.toLowerCase().split(" ").map {
            it.capitalize()
        }.joinToString(" ")
}
```
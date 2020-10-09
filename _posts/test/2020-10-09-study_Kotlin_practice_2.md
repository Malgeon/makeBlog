---
layout: post
author: test
title:  "연습문제 : 최댓값과 최솟값"
description: "연습문제"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12939"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 최댓값과 최솟값</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


문자열을 다루는 간단한 문제이지만, 표준함수를 사용하면 더욱 간단해진다.

```java
class Solution {
    fun solution(s: String): String {
        var arr = s.split(" ").map { it.toInt() }
        return "${arr.min()} ${arr.max()}"
    }
}
```


```java
class Solution {
    fun solution(s: String): String = s.split(" ").map { it.toInt() }.let { "${it.min()} ${it.max()}" }
}
```
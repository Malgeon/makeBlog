---
layout: post
author: test
title:  "연습문제 : 피보나치 수"
description: "연습문제"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12945"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 피보나치 수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


숫자가 Int 범위보다 클 경우에 대한 이슈를 잘 생각해 놔야 한다.

끝 값에 filter를 하는 것이 아닌 중간 값에 filter를 해야 한다.


```java
class Solution {
    fun solution(n: Int): Int {
        var ans = Array(n+1) { 0 }
        ans[1] = 1
        for(i in 2..n) ans[i] = (ans[i-1] + ans[i-2])%1234567
        return ans[n]
    }
}
```

재귀 풀이

```java
class Solution {
    fun solution(n: Int): Int = if(n == 1 || n == 2) 1 else fib(n, 1, 1)  
    tailrec fun fib(n: Int, a: Int, b: Int): Int = if(n == 1) a else fib(n - 1, b % 1234567, (a + b) % 1234567)
}
```
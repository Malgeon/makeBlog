---
layout: post
author: test
title:  "논리연산자 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

### 논리 연산자 - and

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12937"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 짝수와 홀수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

전형적인 문제풀이

```java
class Solution {
    fun solution(num: Int) = if(num % 2 == 0) "Even" else "Odd"
}
```

and operator를 사용하여 해결한다. 여기서 and는 논리연산 and를 의미하며 비트연산으로 판단한다.

```java
class Solution {
    fun solution(num: Int): String = if (num and 1 == 1) "Odd" else "Even"
}
```

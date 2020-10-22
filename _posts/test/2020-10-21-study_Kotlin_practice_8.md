---
layout: post
author: test
title:  "2017 팁스다운 : 예상 대진표"
description: "수식 문제"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12985"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2017 팁스다운 : 예상 대진표</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

단순 수식 문제이다. 그러나 개인적으로 처음엔 문제를 자세히 보지 않아서 그런지 헤맸다.
아래의 문제 해결 코드에서 굳이 run을 사용하지 않아도 되지만, 그냥 한번 써봤다.

```java
class Solution {
    fun solution(n: Int, a: Int, b: Int): Int = run {
        var count = 0
        var tmpA = a
        var tmpB = b

        while (tmpA != tmpB) {
            tmpA = (tmpA + 1) / 2
            tmpB = (tmpB + 1) / 2
            count++
        }
        count
    }
}
```
---
layout: post
author: test
title:  "연습문제 : N개의 최소공배수"
description: "수식 문제"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12949"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : N개의 최소공배수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

처음에는 전체를 곱한 뒤, 최대공약수로 n-1번 나눠주는 것을 문제 해결 방향으로 잡았다.
그러나 이것은 테스트 케이스에서는 통과 하지만 정확히 맞지 않는 방법이기 때문에 실행 단계에서 전체 다 틀리게 된다. (테스트 케이스에만 국한해서 생각하지 말자.)

순서대로 최소공배수를 구해가는 것이 정확한 문제 해결방법이다.

```java
class Solution {
    fun solution(arr: IntArray) = arr.reduce { acc, i ->
        acc * i / gcd(acc, i)
    }
    private fun gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
}
```
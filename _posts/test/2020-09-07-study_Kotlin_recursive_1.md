---
layout: post
author: test
title:  "Recursive 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

### 재귀

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12943"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 콜라츠 추측</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

피보나치 수열과도 같은 문제.
반복문을 이용한 방법과 재귀를 이용한 방법이 있다.

반복문 : value값을 바꿔야 하기 때문에, 새로 설정을 해주고 그것을 기준으로 반복


```java
class Solution {
    fun solution(num: Int): Int {
        var answer = 0
        var value:Long = num.toLong()
        while(value != 1L) {
            if(answer >500) return -1
            when (value%2L) {
                0L -> value /= 2L
                else -> value = value * 3L + 1L
            }
            answer++
        }
        return answer
    }
}
```

재귀 : 꼬리 재귀 함수를 이용해 해결

```java
class Solution {
    fun solution(num: Int): Int = collatzAlgorithm(num.toLong(),0)

    tailrec fun collatzAlgorithm(n:Long, c:Int):Int =
        when{
            c > 500 -> -1
            n == 1L -> c
            else -> collatzAlgorithm(if( n%2 == 0L ) n/2 else (n*3)+1, c+1)
        }
}
```
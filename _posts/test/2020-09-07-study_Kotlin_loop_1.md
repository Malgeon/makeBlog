---
layout: post
author: test
title:  "반복문 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

### 반복문 

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12912"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 두 정수 사이의 합</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

두 정수 a, b 중 큰 값을 찾고, 순서대로 더해주는 쉬운 문제
a와 b가 같다면 굳이 for문에 넣지 말고 바로 return 해주자.

```java
class Solution {
    fun solution(a: Int, b: Int): Long {
        var answer:Long = 0
        val max:Int
        val min:Int

        if (a>b) {max=a; min=b} else if (a<b) {max=b; min=a} else return a.toLong()

        for (i in min..max) {
            answer += i
        }
        return answer
    }
}
```


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12928"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 약수의 합</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

전형적인 문제풀이

```java
class Solution {
    fun solution(n: Int): Int {
        var answer = 1
        if (n <= 1) return n
        for( i in 2..n/2) {
            if(n%i == 0) {
                answer += i
            }
        }
        return answer+n
    }
}
```

배열과 filter를 이용해서 문제를 해결할 수 있다.
이때, n 이 1일경우 (1..0.5) 이기 때문에 answer는 0이다.

```java
class Solution {
    fun solution(n: Int): Int {
        var answer = 0
        answer = (1..n/2).filter { n % it == 0 }.sum()
        return answer
    }
}
```
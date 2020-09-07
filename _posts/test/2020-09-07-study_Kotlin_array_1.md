---
layout: post
author: test
title:  "배열 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---


### 기본적인 배열 선언

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12901"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 2016년</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

내 풀이

```java
class Solution {
    fun solution(a: Int, b: Int): String {
        var answer = ""
        var monthArr = intArrayOf(31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
        var day = 0
    
        var monthCal = a-1
        var count = 0;
        while (monthCal>0) {
            day += monthArr[count]
            count++
            monthCal--
        }
        day += b-1
        var weekend = day % 7
    
        when(weekend) {
            1 -> answer = "SAT"
            2 -> answer = "SUN"
            3 -> answer = "MON"
            4 -> answer = "TUE"
            5 -> answer = "WED"
            6 -> answer = "THU"
            0 -> answer = "FRI"
        }
        return answer
    }
}
```

다른사람 풀이

```java
class Solution {
    fun solution(a: Int, b: Int): String {        
        val month = listOf(31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
        val dayOfWeek = listOf("FRI", "SAT", "SUN", "MON", "TUE", "WED", "THU")
        var diff = 0

        for (i in 0 until a - 1) {
            diff += month[i]
        }

        diff += (b - 1)

        return dayOfWeek[diff%7]
    }
}
```

- 1월 1일이 고정이라는 조건을 생각한다면, 위와 같이 단순히 for문을 통해서 월에 해당하는 일수를 더해줄 수 있다. 그리고 그런 방식의 코딩이 더 깔끔해 보이긴 하다.


### 배열 forEach

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12950"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 행렬의 덧셈</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

우선 새로 배열을 만들고, 해당 배열의 값을 직접 넣어주는 방법이 있다.

```java
class Solution {
    fun solution(arr1: Array<IntArray>, arr2: Array<IntArray>): Array<IntArray> {
        var answer = Array<IntArray>(arr1.size,{ IntArray(arr1.get(0).size,{0})})
        for(i in 0 until arr1.size) {
            arr1[i].forEachIndexed { j, it ->
                var num = it + arr2[i].get(j)
                answer[i].set(j, num)
            }
        }
        return answer
    }
}
```

Array class를 사용해서 생성할 때, 값을 직접 넣어주는 방법이 있다.

```java
class Solution {
    fun solution(arr1: Array<IntArray>, arr2: Array<IntArray>): Array<IntArray> {
        return Array(arr1.size) { row ->
            IntArray(arr1[0].size) { col ->
                arr1[row][col] + arr2[row][col]
            }
        }
    }
}
```

### 배열 - 람다식을 이용한 초기화

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12954"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : x만큼 간격이 있는 n개의 숫자</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

람다식을 이용한 배열 초기화 기억해두자.

```java
class Solution {
    fun solution(x: Int, n: Int) = LongArray(n) { it -> x.toLong() * (it + 1) }
}
```
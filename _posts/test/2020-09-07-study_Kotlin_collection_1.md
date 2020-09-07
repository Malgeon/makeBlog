---
layout: post
author: test
title:  "Collection 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

### Collection List

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12910"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 나누어떨어지는 숫자 배열</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

2가지 방법을 생각하였다.
- arr에서 순환하여 조건에 맞는 값을 빈 공간에 추가하기
- arr로부터 filter를 이용해 배열을 만들기

그런데 2가지 방법 모두 Collection을 이용해야 하므로 List에서만 사용이 가능하다.
따라서 List에서 처리한후 toIntArray를 이용해 return함으로써 해결하였다.


```java
class Solution {
    fun solution(arr: IntArray, divisor: Int): IntArray {
        var answer = ArrayList<Int>()
        for (value in arr) {
            if (value%divisor == 0) {
                answer.add(value)
            }
        }
        if (answer.isEmpty()) answer.add(-1)
        return answer.toIntArray().sortedArray()
    }
}
```
```java
class Solution {
    fun solution(arr: IntArray, divisor: Int): IntArray {
        var answer = arr.filter{ it % divisor == 0 }
        if (answer.isEmpty()) return intArrayOf(-1)
        else return answer.toIntArray().sortedArray()
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12935"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 제일 작은 수 제거하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

작은수를 찾고, 해당 index를 알아서 빼줘야하는 고민을 하고 있엇는데, filter기능을 이해하면 한번에 해결이 가능하다.

```java
class Solution {
    fun solution(arr: IntArray): IntArray {
        if(arr.size == 1) return intArrayOf(-1)
        else return arr.filter{ it != arr.min() }.toIntArray()
    }
}
```

### Collection 확장함수 fold

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12947"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 평균 구하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

```java
class Solution {
    fun solution(x: Int) =  x % x.toString().map{it.toInt() - '0'.toInt()}.toIntArray().sum() == 0
}
```

[fold]((../study_kotlin_33))를 사용할 수 있다.

```java
class Solution {
    fun solution(x: Int) =  x % x.toString().fold(0){acc, v -> acc + v.toInt() - '0'.toInt()} == 0
}
```

---
layout: post
author: test
title:  "연습문제 : 행렬의 곱셈"
description: "배열 연산 연습"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12949"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 행렬의 곱셈</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

여러가지 방법 중 3 가지를 작성하고자 한다.

1. Array 선언부의 람다식을 이용한 방법
2. 빈 Array를 선언하여, also와 fold를 이용한 방법
3. map을 이용하여 매개 arr의 index를 이용한 방법

<br>

#### 1번 방법 
Array 선언부의 (Int)는 index를 가지고 있으므로 해당 index를 이용해 계산 후 value를 return 하는 방법이다.

```java
class Solution {
    fun solution(arr1: Array<IntArray>, arr2: Array<IntArray>) = Array<IntArray>(arr1.size) { i -> 
        Array<Int>(arr2[0].size) { j ->
                var value = 0
                for(n in 0 until arr1[i].size) {
                    value += (arr1[i][n] * arr2[n][j])
                }
                value
        }.toIntArray()
    }
}
```


#### 2번 방법
빈 배열을 선언하고 해당 빈 배열에서 also를 이용하여 배열에 값을 집어 넣은 후 배열 return

```java
class Solution {
    fun solution(arr1: Array<IntArray>, arr2: Array<IntArray>) = Array(arr1.size) {
        IntArray(arr2.first().size) }.also {
            for (n in arr1.indices)
                for (m in arr2.first().indices)
                    it[n][m] = arr2.indices.fold(0) { acc, i -> acc + arr1[n][i] * arr2[i][m] }
    }
}
```


#### 3번 방법
빈 배열을 생성하기 보다 어차피 return 배열의 크기는 mapIndexed와 fodIndexed에서 이용할 수 있으므로 fold를 이용하여 값을 계산후 return 해준다.

```java
class Solution {
    fun solution(arr1: Array<IntArray>, arr2: Array<IntArray>) = arr1.map { row ->
            arr2[0].mapIndexed { index, a ->
                row.foldIndexed(0) { idx, acc , i ->
                    acc + i * arr2[idx][index]
                }
            }.toIntArray()
    }.toTypedArray()
}
```

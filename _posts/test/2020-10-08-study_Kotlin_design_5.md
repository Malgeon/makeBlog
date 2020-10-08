---
layout: post
author: test
title:  "설계 : 튜플"
description: "2019 카카오 개발자 겨울 인턴십"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/64065"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2019 카카오 개발자 겨울 인턴십 : 튜플</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


2가지 방법이 존재한다.

1. 요소를 List로 저장을 한 뒤, 크기 순으로 정렬하여 튜플 출력
2. 요소의 원소를 groubBy하여 List로 저장을 한 뒤, 개수가 많은 순서대로 정렬하여 튜플 출력

방법 자체는 간단한데, 이것을 구현하기 위한 과정을 잘 기억해야 한다.


- 첫번째 방법
    - 요소를 저장하기 위한 기준으로 `},{`을 잡아 각각의 값 을 저장하되 Int형으로 저장하기 위해 한번더 map처리를 한다. 
    - 이 때, 연산속도를 높이기 위해 asSequence를 이용해 병렬적으로 처리를 해준다.
    - asSequence의 결과값인 TransformingSequence를 .toList()를 통해 List로 바꾼뒤 size로 정렬을 해준다.
    - 튜플은 크기순으로 정렬된 요소들의 합집합이므로 Set의 operation인 union을 이용하여 구해준다.

```java
class Solution {
    fun solution(s: String): IntArray {
        return s.substring(2 until s.length-2)
            .split("},{")
            .asSequence()
            .map { it.split(",").map { num -> num.toInt() } }
            .toList()
            .sortedBy { it.size }
            .fold(setOf<Int>()) { acc, list -> acc.union(list) }
            .toIntArray()
    }
}
```

- 두번째 방법
    - 숫자 String이 아닌 값을 기준으로 모두 나눈 뒤 groupBy를 이용해 저장한다.
    - 이 떄, 해당 값은 List<Pair<String, List<String>>> 이므로 second의 크기를 기준으로 정렬을 해준다.
    - 위에서 저장한 값은 자연스레 "" 값이 가장 많이 존재한다. 따라서 첫번째 값을 제외하기 위해 takeLast를 사용한다.
    - 그리고 apply를 이용하 arrayListOf의 함수를 사용하고 있으므로, add 값을 저장하여 답을 구해준다.


```java
class Solution {
    fun solution(s: String): IntArray = arrayListOf<Int>().apply {
        s.split("{", "}", ",")
            .groupBy { it }
            .toList()
            .sortedByDescending { it.second.count() }
            .also {
                it.takeLast(it.size - 1)
                    .forEach { add(it.first.toInt()) }
            }
    }.toIntArray()
}
```







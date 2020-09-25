---
layout: post
author: test
title:  "정렬 : H-Index"
description: "연습 문제"
categories: [ test ]
tags: [ sort ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42747"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">정렬 : H-Index</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


문제 설명에 따르면 논문 n편 중, h편 이상 인용된 논문이 h편 이상인 h의 최대값이므로
h값은 0 부터 n 사이에 존재한다.

따라서 0편 부터 n편까지 순서대로 올라가면서 해당 조건에 맞는 값을 찾으면 된다.

여기선 정확도만을 측정해서 굳이 이분탐색으로 해결하지는 않았다.

```java
class Solution {
    fun solution(citations: IntArray): Int {
        var answer = 0
        for( i in 0 until citations.size+1) {
            if(i > citations.filter { it >= i }.count() ) {
                break
            }
            answer = i
        }
        return answer
    }
}
```
---
layout: post
author: study
title:  "Kotlin 코딩테스트 메모장"
description: "문제풀며 알아둬야할 정보들을 찾아보자."
categories: [ study ]
postImgOn: true
tags: [programming, kotlin]
image: assets/images/study/kotlin.png
insertUrlImg: assets/images/study/programmers.png
---

```
/******************************************************************************/
try {
    ...
} catch ( e: CancellationExceoption) {ㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁ
    ... 취소에 대한 예외처리 ...
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12901"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 2016년</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

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
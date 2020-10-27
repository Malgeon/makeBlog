---
layout: post
author: test
title:  "2019 KAKAO BLIND RECRUITMENT : 오픈채팅방"
description: "문자열 다루기"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42888"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2019 KAKAO BLIND RECRUITMENT : 오픈채팅방</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


여타 문제가 다들 그렇지만 역시 풀이 과정에서는 문제 해결 방향 설정하는 것이 어렵다.
이번 문제 또한 그러했다. 

풀이 과정에서 가장 핵심적인 단서는

1. 닉네임은 유저아이디와 연결이 된다.
2. 닉네임이 바뀌는 순간은 Enter와 Change의 명령어가 있을 경우이다.

이 두 가지인데, 이 단서를 가지고 바뀌는 마지막 닉네임을 map을 이용하여 저장하도록 한 뒤, return 문을 작성하도록 하였다.

```java
class Solution {
    fun solution(record: Array<String>): Array<String> {
        var answer = mutableListOf<String>()
        var rtnAnswer = mutableMapOf<String, String>()
        val recordList = record.map {
            it.split(" ")
        }

        recordList.forEach {
            if (it.size == 3) rtnAnswer[it[1]] = it[2]
        }

        recordList.forEach {
            if (it[0] == "Enter") answer.add("${rtnAnswer[it[1]]}님이 들어왔습니다.")
            else if (it[0] == "Leave") answer.add("${rtnAnswer[it[1]]}님이 나갔습니다.")
        }
        return answer.toTypedArray()
    }
}
```
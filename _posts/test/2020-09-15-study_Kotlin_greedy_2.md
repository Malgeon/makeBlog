---
layout: post
author: test
title:  "탐욕법 : 조이스틱"
description: "연습 문제"
categories: [ test ]
tags: [ greedy ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42860"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">탐욕법(Greedy) : 조이스틱</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


이번 문제를 풀어보고 난 후, Greedy 문제는 코딩테스트에서 쉽게 문제로 제출되기 어렵다는 결론을 내게 되었다.

이 경우에서 해당 알고리즘이 최적해를 보장하지 않기 떄문인데, 어디까지를 정답으로 할지 애매하다.

우선 조이스틱의 위 아래에 소요되는 결과값은 동일한데, 특정 조건에서 죄로 움직일지, 우로 움직일지 구분하고자 할때
이 문제의 정답은 default로 오른쪽으로 이동하면서 'A'란 문자가 등장하면 다음 'A' 나올때 까지의 거리를 잰후 해당 거리가 왼쪽으로 이동할 떄 소요되는 거리와 비교하여 나오는 값중 
가장 작은 거리를 추가해 주면 된다. (어째서 이게 정답인지는 잘 모르겠지만 일단 정답은 그러하다.)
그래서 해당 답안에 아래와 같은 사례를 대입해보면 최적해가 아니라는 것은 금방 알 수 있다.

```java
class Solution {
    fun solution(name: String): Int {
        var answer = 0
        val updownDiff = intArrayOf(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1)
        name.forEach {
            answer += updownDiff[it - 'A']
        }
        return answer + findMinPoint(name)
    }

    fun findMinPoint(name: String): Int {
        var rowPoint = name.length-1
        for(i in name.indices) {
            if(name[i] == 'A') {
                var nextIdx = i+1
                var countA = 0

                while(nextIdx < name.length && name[nextIdx] =='A') {
                    nextIdx++
                    countA++
                }
                val left = (i -1) * 2 + (name.length -1 -i -countA)
                rowPoint.coerceAtMost(left)
            }
        }
        return rowPoint
    }
}
```

아닌 사례

```
BBABA → 6
BBBAAB → 8
BBAABAA → 7
BBAABBB → 10
BBAABAAAA → 7
BBAABAAAAB → 10
```
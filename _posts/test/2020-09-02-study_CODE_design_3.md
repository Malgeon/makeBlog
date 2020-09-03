---
layout: post
author: test
title:  "설계 : 실패율"
description: "2019 카카오 블라인드 리쿠르트"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42889"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 실패율</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

위 문제를 푸는 과정은 다음과 같다.

1. 실패율에 해당하는 값을 계산한다.
2. 실패율값을 배열에 Stage - 실패율값 으로 저장한다.
3. 조건에 맞게 정렬을 한다.
4. IntArray return


우선 실패율을 구하기 위한 방법으로 2가지를 제시하자면 아래와 같다
1. 2중 반복문을 이용하여 pass와 fail의 수를 구하여 실패율 값을 계산
2. stages를 정렬하여 다른 stage가 나오게 하며, 해당 스테이지의 fail을 오름차순으로 올라가면 자연스레 pass한 숫자를 알수 있다.

또한 Stage와 실패율 값을 기록하는 방법으로 2가지를 제시하자면 아래와 같다
1. Pair를 이용하여 값을 Array에 저장
2. data class를 이용하여 값을 Array에 저장

아래에서 확인하자.

- stage를 정렬하여 실패율 계산, Pair를 이용하여 값을 Array에 저장

```java
class Solution {
    fun solution(N: Int, stages: IntArray): IntArray {
        var answer = intArrayOf()
        var failrate = Array(N+2) { Pair(it, 0.0) } 
        
        var stage = 0
        var count = 0.0
        var len = stages.size.toDouble()
        
        for( i in stages.sortedArray() ) {
            if(stage != i ) {
                failrate[stage] = Pair(stage, count/len)
                stage = i
                len -= count
                count = 0.0
            }
            count++
        }
        failrate[stage] = Pair(stage, count/len)
        
        return failrate.sliceArray(1..failrate.size-2)
                       .sortedArrayWith(Comparator<Pair<Int, Double>> { a, b ->
                            when {
                                a.second < b.second -> 1
                                a.second == b.second -> {
                                    when {
                                        a.first > b.first -> 1
                                        else -> -1
                                    }
                                }
                                else -> -1
                            }
        }).map{ it.first }.toIntArray()
    }
}
```

Pair는 여러모로 유용할 것 같다. 
람다식을 이용한 정렬도 가능하며, 각 값을 쉽게 바꿀 수 있으며, 해당 값은 쌍으로 다룰수 있기 때문이다.


- 2중 반복문을 이용하여 실패율을 계산, data class를 이용하여 값을 저장

```java
data class Stage(var level: Int, var pass: Int, var fail: Int) {
    val failRate: Float
    get() = if (fail+pass == 0)  0.0f else (fail.toFloat()) / (pass + fail)
}

class Solution {
    fun solution(N: Int, stages: IntArray): IntArray {
        var stageInfo = Array(N,  { Stage(it+1, 0, 0)})

        for (level in stages) {
            for (i in 0.until(level-1)) {
                stageInfo[i].pass++
            }
            if (level != N+1) stageInfo[level-1].fail++
        }
        stageInfo.sortByDescending { it.failRate }
        return stageInfo.map { it.level }.toIntArray()
    }
}
```

사실 단순히 실패율로만 저장하면 되는 것이 이미 내림차순으로 정렬이 되어 있기 때문에 값이 같다면 내림차순으로 정렬된다.
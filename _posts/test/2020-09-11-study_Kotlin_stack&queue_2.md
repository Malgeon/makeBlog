---
layout: post
author: test
title:  "스택/큐 : 기능개발"
description: "스택 + 배열다루기"
categories: [ test ]
tags: [ stack ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/62048"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">스택/큐 : 기능개발</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>



스택/큐로 구분지어져 있으나, 정확히는 FIFO의 컨셉만 가지고 있는 배열을 다루는 문제라고 생각한다.
그렇게 어렵지 않는 풀이라 문제와 풀이를 읽어보면 바로 이해가 될 것이다.

그것 보다도 해당 문제를 풀어가면서 kotlin에서 배열을 다룰 때, 자바스크립트와 비교해 차이점에 대해 알아보고자 한다.

1. 배열에서 해당 배열의 size를 넘어갈때, 자바스크립트는 undefined를 return 하지만 코틀린은 범위가 넘어간 exception을 발생한다.
따라서 코틀린에서 배열을 while문에서 다룰 때, exception이 넘어가지 않도록 주의를 해야 한다.

2. 자바스크립트의 splice는 코틀린에서 drop으로 기능을 대체할수 있을것 같다.
그러나 drop은 List를 return 하므로 자료형 변환에 주의해야 한다. (물론 단순히 toIntArray()와 같은 함수만 추가해 주면 되는 것이지만)

3. toMutableList()로 mutable한 list로 변환이 가능하다.


```java
class Solution {
    fun solution(progresses: IntArray, speeds: IntArray): IntArray {
        var answer = arrayListOf<Int>()

        var nextStep = progresses.toMutableList()
        var speedsList = speeds.toList()
        while(nextStep.isNotEmpty()) {
            speedsList.forEachIndexed { index, i ->
                nextStep[index] += i
            }

            if(nextStep.first() >= 100) {
                var count = 0

                for(i in nextStep) {
                    if(i<100) break
                    count++
                }

                answer.add(count)
                nextStep = nextStep.drop(count).toMutableList()
                speedsList = speedsList.drop(count)
            }
        }
        return answer.toIntArray()
    }
}
```


### 다른 풀이

진척 속도를 완성되는데 소요되는 날짜를 계산하여 배열로 다시 만든 후 해당 배열의 첫번째 값을 max로 하여 
연속적인 다음 값이 max값보다 큰 개수를 구하며 해당 값을 answer에 넣는다.

```java
class Solution {
   fun solution(progresses: IntArray, speeds: IntArray): IntArray {
        val answer : MutableList<Int> = arrayListOf()
        val days = progresses

        for(i in 0 until progresses.size){
            days[i] = ((100 - progresses[i])/speeds[i])
        }

        var max = days[0]
        var releaseCount = 1

        for(i in 1 until days.size){
            if(max < days[i]){
                answer.add(releaseCount)
                releaseCount = 1
                max = days[i]
            }else{
                releaseCount ++
            }
        }

        answer.add(releaseCount)
        return answer.toIntArray()
    }
}

```
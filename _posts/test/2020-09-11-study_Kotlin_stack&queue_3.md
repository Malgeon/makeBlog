---
layout: post
author: test
title:  "스택/큐 : 다리를 지나는 트럭"
description: "큐 + 배열 다루기"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42583"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">스택/큐 : 다리를 지나는 트럭</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


스택/큐로 구분지어져 있으나, 나는 이것을 배열로 풀었다.

큐를 사용했더라면 더욱 쉽게 풀었을 것이다.

문제 해결을 위해 고민했던 지점은 다음과 같다.
1. 대기 트럭에서 다리를 건너는 트럭 배열로 옮길 때, 시간당 소요되는 다리 길이 처리 방법
    - Pair에 트력 무게와, 다리 길이를 함께 저장하고, 저장된 다리 길이는 타이밍마다 1씩 줄어들도록 했다.
2. 대기 트럭 -> 다리를 건너는 트럭, 다리를 건너는 트럭 -> 다리를 지난 트럭 으로 옮길때. 순서대로 처리 하는 방법
    - 순서를 맞추면서 Bound에서 벗어난 exception를 isNotEmpty로 막고자 하였다.


```java
class Solution {
    fun solution(bridge_length: Int, weight: Int, truck_weights: IntArray): Int {
        var answer = 0

        var waitingTruck = truck_weights.toMutableList()
        var leftedTruck = arrayListOf<Int>()
        var movingTruck : MutableList<Pair<Int,Int>> = arrayListOf()
        val size = truck_weights.size
        var currentWeight = 0
        var truckIndex = 0

        while(leftedTruck.size < size) {
            movingTruck = movingTruck.map { Pair(it.first, it.second-1)}.toMutableList()
            if(movingTruck.isNotEmpty()) {
                if(movingTruck.first().second == 0) {
                    leftedTruck.add(movingTruck.first().first)
                    movingTruck = movingTruck.drop(1).toMutableList()
                }
            }
            if(waitingTruck.isNotEmpty()) {
                if(movingTruck.sumBy { it.first } + waitingTruck.first() <= weight) {
                    movingTruck.add(Pair(truck_weights[truckIndex++], bridge_length))
                    waitingTruck = waitingTruck.drop(1).toMutableList()
                }
            }
            answer++
        }
        return answer
    }
}
```


### 더 나은 풀이

이 문제를 쉽게 해결하기 어렵게 하는 부분은 다리를 건너는 동안 경과 시간이 지나야 하며 다리를 건너는 동작과 다리를 지나는 동작은 먼저 다리를 건너고, 대기 트럭에서 다리를 건너러 와야하는 순서가 존재한다는 점이다.

그래서 기존 풀이에서는 새로 배열을 만들어 다리를 건너는 경과 정보를 지니게 했지만 그것은 로직을 더욱 복잡하게 한다.

여기서는 그것보다 하나의 큐를 다리를 건너는 트럭의 정보를 가지게 하며, 조건 상 다리에 들어오지 못할 경우 해당 경과를 간직하게 하기 위해 무의미한 동작을 하나 추가해 주는 방법으로 해결하였다.
무의미한 동작을 0으로 하는 이유는 다리 내의 무게를 파악하고자 할 때, 영향을 주지 않게 하기 위함이다.

```java
class Solution {
    fun solution(bridge_length: Int, weight: Int, truck_weights: IntArray): Int {
        var answer = 0
        val q : Queue<Int> = LinkedList()
        var present = 0
        var last = 0
        var passed = truck_weights.size
        if(bridge_length == 1)
            return truck_weights.size
        for(i in 0..bridge_length-1){
            q.add(0)
        }
        while(last < truck_weights.size){
            answer++
            present -= q.remove()
            if(present + truck_weights[last] <= weight){
                q.add(truck_weights[last])
                present += truck_weights[last]
                last++
            }
            else
                q.add(0)
        }
        return answer+bridge_length;
    }
}
```

해당 대기 트럭 또한 Queue로 만들어 다루게 되면 truck_weights의 빈 배열 인덱스의 위험을 줄일 수 있다.
또한 해당 queue 배열 내의 sum() 함수를 사용하면 불 필요한 변수를 추가하지 않아도 되게 된다.

```java
class Solution {
    fun solution(bridge_length: Int, weight: Int, truck_weights: IntArray): Int {
        var answer = 0
        val bridgeQueue: Queue<Int> = LinkedList(List(bridge_length){0})
        val waitingQueue: Queue<Int> = LinkedList(truck_weights.toList())

        while (bridgeQueue.isNotEmpty()) {
            answer++
            bridgeQueue.poll()
            if (waitingQueue.isNotEmpty()) {
                if (bridgeQueue.sum() + waitingQueue.peek() <= weight) {
                    bridgeQueue.add(waitingQueue.poll())
                } else {
                    bridgeQueue.add(0)
                }
            }
        }
        return answer
    }
}

```
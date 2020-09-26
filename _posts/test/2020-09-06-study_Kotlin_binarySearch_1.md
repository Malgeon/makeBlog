---
layout: post
author: test
title:  "이분 탐색 : 징검다리 건너기"
description: "2019 카카오 개발자 겨울 인턴십"
categories: [ test ]
tags: [ binary ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
testFeatured: true
hidden: true
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/64062"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2019 카카오 개발자 겨울 인턴십 : 징검다리 건너기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.] = 완전탐색으로 풀면 0 점이라는 소리다.
보통 이런 문제의 가장 큰 해결 방안 중 하나는 이분 탐색이다.

그렇지만 문제에서 설명하는 대로 문제를 해결하다 보면 방법이 결국에는 완전탐색쪽으로 떠오른다. <br>

이 문제를 살펴보면 stone의 값이 결국 걸어갈 수 있는 인원을 나타내므로 stone의 가장 적은 값부터 빼주되 처음 0이하가 되는 index를 true로 하는 배열을 가질 수 있다. <br>
이 배열에서 true를 가지는 index의 연속적인 개수가 k값보다 커지는 직후의 순간을 찾아내도록 하는 방법이 하나의 방법이 될수 있는데, 이는 완전탐색이다. <br>

아래가 위의 방법으로 풀어본 예시<br>
아래에선 stones의 값을 중복을 제거 한후 내림차순 정렬을 하게 되면 해당 값들이 각각의 배열 안에서 0이 되는 값이기 때문에
그 값의 index를 찾아주면 k값을 검사할 수 있게 된다.

```java
class Solution {
    fun solution(stones: IntArray, k: Int): Int {
        val checkStoneSet = BooleanArray(stones.size) { false }
        val stoneSet = mutableSetOf<Int>()
        stones.forEach { stoneSet.add(it) }
        var sortedStoneSet = stoneSet.sorted()
        var curStone = 0

        curStone = sortedStoneSet.first()
        sortedStoneSet = sortedStoneSet.drop(1)

        while (sortedStoneSet.isNotEmpty()) {
            stones.forEachIndexed{ index, i ->
                if(i==curStone) {
                    checkStoneSet[index] = true
                }
            }
            if( checkStone(checkStoneSet, k)) {
                return curStone
            }
            curStone = sortedStoneSet.first()
            sortedStoneSet = sortedStoneSet.drop(1)
        }
        return curStone
    }
}


fun checkStone(stonesList: BooleanArray, k: Int): Boolean {
    var now = 0
    for( i in stonesList) {
        if(i) now += 1
        else now = 0
        if(now >= k) return true
    }
    return false
}
```

- 결과 : 정확성 100% 효율성 0%

위와 같은 이해를 바탕으로 더 빨리 풀수 있는 방법(스킬)을 찾아야 하는데, 결론적으로 찾은 방법은 이분탐색이다.

방법을 구체적으로 설명하자면
1. 구하고자 하는 값을 어떤 로직의 결과로 도출이 내는 것이 아닌, 값을 특정한 다음 해당 값이 조건에 일치한지 검사한다.
2. 해당 조건에서는 값이 맞는지 틀리는지와 함께 틀리다면 해당 결과값이 조건에 맞는 값과 비교 할수 있으며, 해당 값과 비교하여 크기 여부를 확인한다.
3. 예상값이 크다면 예상값을 작게, 작다면 예상값을 크게한다. 그렇게 하는 방법은 범주를 정하고 양 끝값의 절반을 예상값으로 한다.(이렇게 이분법으로 나누니 때문에 이분탐색)
4. 그러나 이 문제에서는 조건에 맞는 값이 여러개가 존재하기 때문에 위 단계에서 추가적으로 조건에 맞는 값이더라도 해당 값중 가장 낮은값을 찾는 upper bound를 로써 문제를 풀어나간다.

아래가 해당 방법으로 푼 예시 <br>
아래에선 값을 예상할 때, 위에서 완전탐색으로 푼 문제를 통해 결국에는 값이 stones에서 나올수 밖에 없기 때문에 stones의 중복값을 제거하고 정렬을 한 뒤
index 이분탐색을 진행하였다. 물론 러프하게 이분탐색을 진행해도 된다.

```java
class Solution {
    fun solution(stones: IntArray, k: Int): Int {
        val stoneSet = mutableSetOf<Int>()
        stones.forEach { stoneSet.add(it) }
        var sortedStoneSet = stoneSet.sorted()
        
        var right = sortedStoneSet.size-1
        var left = 0
        
        var mid: Int
        
        while (left <= right) {   
            mid = (left+right)/2
            
            if(checkStone(stones, sortedStoneSet[mid], k)) right = mid-1
            else left = mid+1
        }
        return sortedStoneSet[left-1]
    }
}

fun checkStone(stones: IntArray, mid: Int, k: Int): Boolean {
    var now = 0
    for( i in stones) {
        if(i < mid) now += 1
        else now = 0
        if(now >= k) return true
    }
    return false
}
```





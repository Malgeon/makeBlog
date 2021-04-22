---
layout: post
author: test
title:  "2021 KAKAO BLIND RECRUITMENT : 순위 검색"
description: "멱집합, 이분탐색"
categories: [ test ]
tags: [ standard, binary ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/72412"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2021 KAKAO BLIND RECRUITMENT : 순위 검색</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

정확성으로만 문제를 풀겠다면 각 조건에 맞게 필터링된 값을 구한 다음 갯수를 구해주면 되니까 간단하다.

그러나 이는 효율성에서 0점을 맞는 답안이기 때문에, 효율성에 맞게끔 문제를 풀어보자.

사실 문제 해결을 위한 Key를 찾기가 어려워 도움을 받아본 결과 다음과 같은 사실을 알게되었다.

1. info의 항목은 고정값이기 때문에 하나의 info String을 '-' 조건을 가진 info문이 만들어 질수 있다.
2. hash를 이용해 조건에 의한 결과값이 key로써 같게 되면 key에 대한 value를 리스트로 추가를 함으로써 해당 점수를 간직한다.

그렇게 되면 query문과 일치한 점수리스트를 바로 얻어올수 있게 되는 것이다.

여기서도 검색이 느려질수 있으므로, sort 뒤 binary lower bound를 이용한다.


정답률 44.07% , 4.49%

문제 푸는 입장에서 난감한게 일단 효율성을 포기하고 정확성으로 문제를 풀고, 나중에 효율성까지 풀고자 한다면 사실상 시간 소요가 2문제를 해결한 것과 다름없게 된다.

이것을 패스하거나 처음부터 효율성으로 문제 해결방향을 잡는 것으로 문제를 풀어야 할 것 깉다.


```java
class Solution {
    fun solution(info: Array<String>, query: Array<String>) = FindResult(info).solve(query)
}

class FindResult(private val infos: Array<String>) {
    private val hashInfo = HashMap<InfoClass, MutableList<Int>>()

    data class InfoClass(
        val lan: String,
        val job: String,
        val level: String,
        val soul: String
    )

    init {
        makingHash()
        insertHash()
        sortHash()
    }

    private fun makingHash() {
        val language = listOf("java", "cpp", "python", "-")
        val position = listOf("backend", "frontend", "-")
        val career = listOf("junior", "senior", "-")
        val soulFood = listOf("pizza", "chicken", "-")

        for (l in language){
            for (p in position){
                for (c in career){
                    for (s in soulFood){
                        hashInfo[InfoClass(l, p, c, s)] = mutableListOf()
                    }
                }
            }
        }
    }

    private fun makeInfo(data: String, infoOrQuery: Boolean): Pair<InfoClass, Int> {
        val split = if(infoOrQuery) data.split(" ") else data.split(" and ", " ")
        val score = if(split[4] == "-") 0 else split[4].toInt()
        return Pair(InfoClass(split[0], split[1], split[2], split[3]), score)
    }

    private fun getPowerSet(arr: IntArray): Array<IntArray> {
        var set = arrayListOf<IntArray>()

        val pivot = arr.size
        val powerSet = 1 shl pivot

        for (i in 1 until powerSet) {
            var oneSet = mutableListOf<Int>()
            for(item in 0 until pivot) {
                if ((i and (1 shl item)) > 0) {
                    oneSet.add(arr[item])
                }
            }
            set.add(oneSet.toIntArray())
        }
        return set.toTypedArray()
    }

    private fun makeHyphen(input: String, flag: Boolean) = if(flag) "-" else input

    private fun makeQueryList(info: InfoClass, position: IntArray): InfoClass {
        var flag = arrayListOf(false, false, false, false)
        position.forEach {
            flag[it-1] = true
        }
        return InfoClass(makeHyphen(info.lan,flag[0]), makeHyphen(info.job,flag[1]),makeHyphen(info.level,flag[2]),makeHyphen(info.soul,flag[3]))
    }

    private fun insertHash() {
        val powerSet = getPowerSet(intArrayOf(1, 2, 3, 4))

        infos.forEach { info ->
            var infoPair = makeInfo(info, true)

            hashInfo[infoPair.first]?.add(infoPair.second)

            powerSet.forEach { flag ->
                val hashKey = makeQueryList(infoPair.first, flag)
                hashInfo[hashKey]?.add(infoPair.second)
            }
        }
    }

    private fun sortHash() {
        for (i in hashInfo) {
            hashInfo[i.key]?.sort()
        }
    }

    private fun lowerBound(arr: MutableList<Int>, target: Int): Int{
        var start = 0
        var end = arr.size

        while (start < end){
            val mid = (start + end) / 2
            if (arr[mid] < target)
                start = mid + 1
            else
                end = mid
        }
        return end
    }

    fun solve(query: Array<String>): IntArray {
        var answer = Array(query.size){it}
        query.forEachIndexed { index, str ->
            val queryInfo = makeInfo(str, false)

            if (hashInfo[queryInfo.first]!!.isNotEmpty()) {
                answer[index] = hashInfo[queryInfo.first]!!.size - lowerBound(hashInfo[queryInfo.first]!!, queryInfo.second)
            } else {
                answer[index] = 0
            }
        }

        return answer.toIntArray()
    }
}
```
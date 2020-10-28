---
layout: post
author: test
title:  "2019 KAKAO BLIND RECRUITMENT : 후보키"
description: "멱집합"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42890"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2019 KAKAO BLIND RECRUITMENT : 후보키</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

이 경우 brute force로 하나씩 검사를 하며, 중복여부 판단가지 겸해서 같이 해줘야 한다.
문제를 처음 보고 판단할 때, 다른 여타 방법이 생각나지 않는다면 보자마자 생각난 brute force 방법이 맞다.
다만 해당 문제는 설계를 효율적으로 해야 하는데, 그러기 위해서는 멱집합에 대한 이해가 필요하다.
또 조건에 맞는 튜플이 Unique한지 여부를 판단할 떄, 튜플의 각 값을 하나의 String으로 만들어 Set에서 관리를 해주면 간단하게 해결이 가능한줄 알았는데..

unique 집합 중 최소성의 조건에 맞춘 값을 찾아내는 것 또한 쉽지 않았다.

추가할 값이 min에 맞는지 확인 하는 작업이 필요하며, "012"는 "02"로 인해 최소성에 조건에 맞지 않아야 하기에 "012"contains("02")는 사용하지 못한다.

이러한 문제를 해결하기 위해, 먼저 최소값이기 때문에 멱집합을 크기가 적은 순서대로 정렬을 한 뒤 조건에 부합한지 들어오는 값을 직접 찾아보는 방법으로 해결을 하였다.


```java
class Solution {
    fun solution(relation: Array<Array<String>>): Int {
        val arr = Array<Int>(relation[0].size) { it }
        var uniquePivot = relation.size
        val arrPowerSet = getPowerSet(arr.toIntArray())
        var uniqueSet = mutableSetOf<String>()

        arrPowerSet.sortedBy { it.size }.forEach { set ->
            var checkUnique = mutableSetOf<String>()
            relation.forEach { item ->
                var value = ""
                set.forEach {
                    value += item[it]
                }
                checkUnique.add(value)
            }
            if(checkUnique.size == uniquePivot) {
                var stringSet = set.joinToString("")
                if(isMin(uniqueSet, stringSet)) uniqueSet.add(stringSet)
            }
        }

        return uniqueSet.size
    }

    private fun isMin(uniqueSet: MutableSet<String>, stringSet: String): Boolean {
        uniqueSet.forEach{
            if(isContains(it, stringSet)) return false
        }
        return true
    }

    private fun isContains(it: String, stringSet: String): Boolean {
        it.forEach {
            if(!stringSet.contains(it)) return false
        }
        return true
    }


    fun getPowerSet(arr: IntArray): Array<IntArray> {
        var visited = Array(arr.size) { false }
        var set = arrayListOf<IntArray>()

        fun addArr(baseArr: IntArray, visited: Array<Boolean>, size: Int) {
            var makeComb = arrayListOf<Int>()
            for(i in 0 until size) {
                if(visited[i]) {
                    makeComb.add(baseArr[i])
                }
            }
            set.add(makeComb.toIntArray())
        }

        fun find(baseArr: IntArray, visited: Array<Boolean>, size:Int, idx: Int) {
            if (idx == size) {
                addArr(baseArr, visited, size)
                return
            }

            visited[idx] = true
            find(baseArr, visited, size, idx + 1)

            visited[idx] = false
            find(baseArr, visited, size, idx + 1)
        }
        find(arr, visited, arr.size, 0)
        return set.toTypedArray()
    }
}
```

그리고 "012"이 "02"으로 인해 최소성에 조건에 맞지 않도록 하는 방법으로 containsAll이 있음을 알게 되었다.
다만 이는 collection 힘수이므로 해당 값을 List로 바꿔서 비교하니 동일한 결과를 얻을 수 있었다.

```java
class Solution {
    fun solution(relation: Array<Array<String>>): Int {
        val arr = Array<Int>(relation[0].size) { it }
        var uniquePivot = relation.size
        val arrPowerSet = getPowerSet(arr.toIntArray())
        var uniqueSet = mutableSetOf<List<Int>>()

        arrPowerSet.sortedBy { it.size }.forEach { set ->
            var checkUnique = mutableSetOf<String>()
            relation.forEach { item ->
                var value = ""
                set.forEach {
                    value += item[it]
                }
                checkUnique.add(value)
            }
            if(checkUnique.size == uniquePivot) {
                val listSet = set.toList()
                if(isMin(uniqueSet, listSet)) uniqueSet.add(listSet)
            }
        }
        return uniqueSet.size
    }

    private fun isMin(uniqueSet: MutableSet<List<Int>>, set: List<Int>): Boolean {
        uniqueSet.forEach{
            if(set.containsAll(it)) return false
        }
        return true
    }

    fun getPowerSet(arr: IntArray): Array<IntArray> {
        var visited = Array(arr.size) { false }
        var set = arrayListOf<IntArray>()

        fun addArr(baseArr: IntArray, visited: Array<Boolean>, size: Int) {
            var makeComb = arrayListOf<Int>()
            for(i in 0 until size) {
                if(visited[i]) {
                    makeComb.add(baseArr[i])
                }
            }
            set.add(makeComb.toIntArray())
        }

        fun find(baseArr: IntArray, visited: Array<Boolean>, size:Int, idx: Int) {
            if (idx == size) {
                addArr(baseArr, visited, size)
                return
            }

            visited[idx] = true
            find(baseArr, visited, size, idx + 1)

            visited[idx] = false
            find(baseArr, visited, size, idx + 1)
        }
        find(arr, visited, arr.size, 0)
        return set.toTypedArray()
    }
}
```

그런데 배열 내 멱집합을 bit계산으로 가능하다. 그렇게 되면 동일하게 bit 계산을 통하여 해당 값의 min에 대한 부합 여부를 쉽게 검사 할 수 있다! 
또한, 코드량도 현저히 줄어든다...!!

```java
class Solution {
    fun solution(relation: Array<Array<String>>): Int {
        val pivot = relation[0].size
        val tupleSize = relation.size
        val powerSet = 1 shl pivot

        var set = arrayListOf<Int>()

        for (i in 1 until powerSet) {
            val checkUnique = mutableSetOf<String>()
            for (row in 0 until tupleSize){
                var value = ""
                for(item in 0 until pivot) {
                    if ((i and (1 shl item)) > 0) {
                        value += relation[row][item]
                    }
                }
                checkUnique.add(value)
            }

            if (checkUnique.size == tupleSize && findMinSet(set,i)) set.add(i)
        }
        return set.size
    }

    private fun findMinSet(set: ArrayList<Int>, item: Int): Boolean {
        set.forEach {
            if((it and item) == it) return false
        }
        return true
    }
}
```



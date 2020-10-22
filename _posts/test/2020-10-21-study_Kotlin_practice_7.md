---
layout: post
author: test
title:  "Summer/Winter Coding(~2018) : 소수 만들기"
description: "수식 문제"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12977"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">Summer/Winter Coding(~2018) : 소수 만들기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

배열에서 3개의 조합을 구성하여 조합의 합이 소수인지를 판단하는 문제이다.
조합을 구성하는 것을 단순히 for문으로 구성할 수 있지만, 여기서는 직접 조합 함수를 이용하였다.

```java
class Solution {
    fun solution(nums: IntArray): Int {
        var answer = 0
        getCombination(nums, 3).forEach {
            if(isPrimeNumber(it.sum())) answer++
        }
        return answer
    }

    fun getCombination(arr: IntArray, num: Int): Array<IntArray> {
        var visited = Array(arr.size) { false }
        var set = arrayListOf<IntArray>()

        fun addArr(baseArr: IntArray, visited: Array<Boolean>, size: Int) {
            var makeComb = arrayListOf<Int>()
            for (i in 0 until size) {
                if (visited[i]) {
                    makeComb.add(baseArr[i])
                }
            }
            set.add(makeComb.toIntArray())
        }

        fun find(baseArr: IntArray, visited: Array<Boolean>, start: Int, size: Int, num: Int) {
            if (num == 0) {
                addArr(baseArr, visited, size)
                return
            } else {
                for (i in start until baseArr.size) {
                    visited[i] = true
                    find(baseArr, visited, i + 1, size, num - 1)
                    visited[i] = false
                }
            }
        }
        find(arr, visited, 0, arr.size, num)
        return set.toTypedArray()
    }

    fun isPrimeNumber(num: Int): Boolean {
        return (2..num / 2).filter { num % it == 0 }.count() == 0
    }
}
```
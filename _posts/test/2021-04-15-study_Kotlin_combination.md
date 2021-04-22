---
layout: post
author: test
title:  "2021 KAKAO BLIND RECRUITMENT : 메뉴 리뉴얼"
description: "조합"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/72411"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2021 KAKAO BLIND RECRUITMENT : 메뉴 리뉴얼</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

주어진 Array를 원하는 course값에 맞게 combination하여 중복 값을 찾는다.
중복 값의 max가 2이상인 Str을 담아 return 해주면 된다.

정답률 25%

간단한 문제라고 생각한다.


```java
class Solution {
    fun solution(orders: Array<String>, course: IntArray): Array<String> {
        var answer = arrayListOf<String>()

        course.forEach { courseNum ->
            var target = makeCousreList(orders, courseNum)
            println(target.toString())
            var max = 0
            target.forEach{
                max = max.coerceAtLeast(it.second)
            }
            var test = target
            if(max>1)  {
                target.filter { it.second == max}.forEach { pairItem ->
                    answer.add(pairItem.first)
                }
            }

        }
        return answer.sorted().toTypedArray()
    }

    fun getCourse(arr: String, num: Int): Array<String> {
        val courseIdx = Array(arr.length) { it }
        var visited = Array(arr.length) { false }
        var courseSet = arrayListOf<String>()

        fun addCourse(picked: Array<Int>, visited: Array<Boolean>, size: Int) {
            var makeCourse = ""
            for(i in 0 until size) {
                if(visited[i]) {
                    makeCourse += arr[picked[i]]
                }
            }
            courseSet.add(makeCourse)
        }

        fun find(picked: Array<Int>, visited: Array<Boolean>, start: Int, size:Int, num: Int) {
            if (num == 0) {
                addCourse(picked, visited, size)
                return
            } else {
                for(i in start until picked.size){
                    visited[i] = true
                    find(picked, visited, i+1, size, num-1)
                    visited[i] = false
                }
            }
        }
        find(courseIdx, visited, 0, arr.length, num)
        return courseSet.toTypedArray()
    }

    fun makeCousreList(orders: Array<String>, courseNum: Int): List<Pair<String, Int>>  {
        var courseSet = mutableSetOf<String>()
        var coursePair = mutableListOf<Pair<String, Int>>()

        val sortedOrders = orders.map{
            String(it.toCharArray().sortedArray())
        }

        sortedOrders.forEach {
            if(it.length == courseNum) {
                if(courseSet.contains(it)) {
                    coursePair[courseSet.indexOf(it)] = Pair(it, coursePair[courseSet.indexOf(it)].second+1 )
                } else {
                    courseSet.add(it)
                    coursePair.add(Pair(it, 1))
                }
            } else if(it.length > courseNum) {
                getCourse(it, courseNum).forEach { separateCusrse ->
                    if(courseSet.contains(separateCusrse)) {
                        coursePair[courseSet.indexOf(separateCusrse)] = Pair(separateCusrse, coursePair[courseSet.indexOf(separateCusrse)].second+1 )
                    } else {
                        courseSet.add(separateCusrse)
                        coursePair.add(Pair(separateCusrse, 1))
                    }
                }
            }
        }
        return coursePair
    }
}
```
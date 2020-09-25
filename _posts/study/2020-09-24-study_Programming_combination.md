---
layout: post
author: study
title:  "조합"
description: "조합을 만들어보자"
categories: [ study ]
tags: [programming, kotlin]
postImgOn: false
image: assets/images/study/kotlin.png
---


### 순열과 조합

순열과 조합은 여러 군데에서 쓰인다.
쓰이는 방법도 다양한데
- 각각의 배열의 값으로써
- 조합된 원소의 값들로써

하지만 설계하는 것이 즉석에서 곧바로 할 수 있는 일이 아니기 때문에, 미리 이해하는 차원에서 만들어 놓고자 한다.


### 결과값 구하기

nCr -> nPr에서 중복 원소를 제외한 개수

- nCr

```java
fun combination(n: Int, r: Int): Int {
    var top = 1
    var bottom = 1

    for(i in 0 until r) {
        top *= n-i
        bottom *= r-i
    }
    return top/bottom
}
```


### nCr 원소 구하기

여러 방법을 제시하고자 한다.

- 입력 String 출력 Array<String>

```java
fun main() {
    var arrString = "ABCD"
    println(getCombination(arrString, 2).contentToString())
}

fun getCombination(arrString: String, num: Int): Array<String> {
    val arr = Array(arrString.length) { it }
    var visited = Array(arrString.length) { false }
    var set = arrayListOf<String>()

    fun addArr(baseArr: Array<Int>, visited: Array<Boolean>, size: Int) {
        var makeComb = ""
        for(i in 0 until size) {
            if(visited[i]) {
                makeComb += arrString[baseArr[i]]
            }
        }
        set.add(makeComb)
    }

    fun find(baseArr: Array<Int>, visited: Array<Boolean>, start: Int, size:Int, num: Int) {
        if (num == 0) {
            addArr(baseArr, visited, size)
            return
        } else {
            for(i in start until baseArr.size){
                visited[i] = true
                find(baseArr, visited, i+1, size, num-1)
                visited[i] = false
            }
        }
    }
    find(arr, visited, 0, arrString.length, num)
    return set.toTypedArray()
}
```

```
[AB, AC, AD, BC, BD, CD]
```

- 입력 : IntArray, 출력 : String

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getCombination(arr, 2).contentToString())
}

fun getCombination(arr: IntArray, num: Int): Array<String> {
    var visited = Array(arr.size) { false }
    var set = arrayListOf<String>()

    fun addArr(baseArr: IntArray, visited: Array<Boolean>, size: Int) {
        var makeComb = ""
        for(i in 0 until size) {
            if(visited[i]) {
                makeComb += baseArr[i]
            }
        }
        set.add(makeComb)
    }

    fun find(baseArr: IntArray, visited: Array<Boolean>, start: Int, size:Int, num: Int) {
        if (num == 0) {
            addArr(baseArr, visited, size)
            return
        } else {
            for(i in start until baseArr.size){
                visited[i] = true
                find(baseArr, visited, i+1, size, num-1)
                visited[i] = false
            }
        }
    }
    find(arr, visited, 0, arr.size, num)
    return set.toTypedArray()
}
```

```
[12, 13, 14, 23, 24, 34]
```


- 입력 : IntArray, 출력 : Array<IntArray>

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getCombination(arr, 2).contentDeepToString())
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
```

```
[[1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4]]
```
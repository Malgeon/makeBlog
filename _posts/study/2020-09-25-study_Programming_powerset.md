---
layout: post
author: study
title:  "멱집합"
description: "멱집합을 구해보자"
categories: [ study ]
tags: [programming, kotlin]
postImgOn: false
image: assets/images/study/kotlin.png
---


### 멱집합

탐색 문제에서 멱집합을 이용해야 하는 경우가 종종 있다.
이해하고 사용하자.


### 부분집합

부분집합을 구성하는 방법은 2가지가 있다.

1. 재귀 :
우선 원소를 사용여부(O/X)로 구분된다.

이는 이진트리로 구현할수 있는데.
단계를 구성하고 각 이진트리에 들어갈때마다 flag에 O,X를 기록한다.
마지막 단계에 들어가게 되면 해당 flag를 바탕으로 해당하는 set을 Array에서 찾고 powerSet에 push한다.


- 입력 String 출력 Array<String>

```java
fun main() {
    var arrString = "ABCD"
    println(getPowerSet(arrString).contentToString())
}

fun getPowerSet(arrString: String): Array<String> {
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

    fun find(baseArr: Array<Int>, visited: Array<Boolean>, size:Int, idx: Int) {
        if (idx == size) {
            addArr(baseArr, visited, size)
            return
        }

        visited[idx] = true
        find(baseArr, visited, size, idx + 1)

        visited[idx] = false
        find(baseArr, visited, size, idx + 1)
    }
    find(arr, visited, arrString.length, 0)
    return set.toTypedArray()
}
```

```
[ 
    ABCD, ABC, ABD, AB, 
    ACD, AC, AD, A, BCD, 
    BC, BD, B, CD, C, D, 
]
```

- 입력 : IntArray, 출력 : String

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getPowerSet(arr).contentToString())
}

fun getPowerSet(arr: IntArray): Array<String> {
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
```

```
[
    1234, 123, 124, 12, 
    134, 13, 14, 1, 234, 
    23, 24, 2, 34, 3, 4, 
]
```


- 입력 : IntArray, 출력 : Array<IntArray>

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getPowerSet(arr).contentDeepToString())
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
```

```
[
    [1, 2, 3, 4], [1, 2, 3], [1, 2, 4], [1, 2], 
    [1, 3, 4], [1, 3], [1, 4], [1], [2, 3, 4], 
    [2, 3], [2, 4], [2], [3, 4], [3], [4], []
]
```


2. bit 연산 :
1을 shl하여 1, 10, 100, 1000 ... 과 같은 값을 구성하여 and 하면 해당 위치에 맞는 값이 나오는지를 확인이 가능하다 이를 O/X로 판단하여 값을 구성할 수 있다. 이때 공집합은 빠진다.

- 입력 String 출력 Array<String>

```java
fun main() {
    var arrString = "ABCD"
    println(getPowerSet(arrString).contentToString())
}

fun getPowerSet(arr: String): Array<String> {
    var set = arrayListOf<String>()

    val pivot = arr.length
    val powerSet = 1 shl pivot

    for (i in 1 until powerSet) {
        var oneSet = ""
        for(item in 0 until pivot) {
            if ((i and (1 shl item)) > 0) {
                oneSet += arr[item]
            }
        }
        set.add(oneSet)
    }
    return set.toTypedArray()
}
```

```
[
    A, B, AB, C, 
    AC, BC, ABC, D, 
    AD, BD, ABD, CD, 
    ACD, BCD, ABCD
]
```

- 입력 : IntArray, 출력 : String

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getPowerSet(arr).contentToString())
}

fun getPowerSet(arr: IntArray): Array<String> {
    var set = arrayListOf<String>()

    val pivot = arr.size
    val powerSet = 1 shl pivot

    for (i in 1 until powerSet) {
        var oneSet = ""
        for(item in 0 until pivot) {
            if ((i and (1 shl item)) > 0) {
                oneSet += arr[item]
            }
        }
        set.add(oneSet)
    }
    return set.toTypedArray()
}
```

```
[
    1, 2, 12, 3, 
    13, 23, 123, 4, 
    14, 24, 124, 34, 
    134, 234, 1234
]
```


- 입력 : IntArray, 출력 : Array<IntArray>

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getPowerSet(arr).contentDeepToString())
}

fun getPowerSet(arr: IntArray): Array<IntArray> {
    var set = arrayListOf<IntArray>()

    val pivot = arr.size
    val powerSet = 1 shl pivot

    for (i in 1 until powerSet) {
        var oneSet = ""
        for(item in 0 until pivot) {
            if ((i and (1 shl item)) > 0) {
                oneSet += arr[item]
            }
        }
        set.add(oneSet)
    }
    return set.toTypedArray()
}
```

```
[
    [1], [2], [1, 2], [3], [1, 3], 
    [2, 3], [1, 2, 3], [4], [1, 4],
    [2, 4], [1, 2, 4], [3, 4], 
    [1, 3, 4], [2, 3, 4], [1, 2, 3, 4]
]
```
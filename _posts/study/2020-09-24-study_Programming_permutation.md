---
layout: post
author: study
title:  "순열"
description: "순열을 만들어보자"
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

nPr -> n개의 경우를 r 번 사용할 경우

- nPr

```java
fun permutaion(n: Int, r: Int): Int {
    var value = 1
    
    for(i in 0 until r) {
        value *= n-i
    }
    return top/bottom
}
```


### nPr 원소 구하기

여러 방법을 제시하고자 한다.

- 입력 String 출력 Array<String>

```java
fun main() {
    var arrString = "ABCD"
    println(getPermutation(arrString, 3).contentToString())
}

fun getPermutation(arrString: String, num: Int): Array<String> {
    var arr = IntArray(arrString.length) { it }
    var visited = Array(arr.size) { false }
    var picked = IntArray(arr.size-1) {0}
    var set = arrayListOf<String>()

    fun addArr(picked: IntArray) {
        var makeComb = ""
        for (i in picked) {
            makeComb += arrString[i]
        }
        set.add(makeComb)
    }

    fun find(baseArr: IntArray, picked: IntArray, visited: Array<Boolean>, depth: Int, size: Int, num: Int) {
        if (depth == num) {
            addArr(picked)
            return
        } else {
            for (i in 0 until size) {
                if(!visited[i]) {
                    visited[i] = true
                    picked[depth] = baseArr[i]
                    find(baseArr, picked, visited, depth + 1, size, num)
                    visited[i] = false
                }
            }
        }
    }
    find(arr, picked, visited, 0, arr.size, num)
    return set.toTypedArray()
}
```

```
[
    ABC, ABD, ACB, ACD, 
    ADB, ADC, BAC, BAD, 
    BCA, BCD, BDA, BDC, 
    CAB, CAD, CBA, CBD, 
    CDA, CDB, DAB, DAC, 
    DBA, DBC, DCA, DCB
]
```

- 입력 : IntArray, 출력 : String

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getPermutation(arr, 3).contentToString())
}

fun getPermutation(arr: IntArray, num: Int): Array<String> {
    var visited = Array(arr.size) { false }
    var picked = IntArray(arr.size-1) {0}
    var set = arrayListOf<String>()

    fun addArr(picked: IntArray) {
        var makeComb = ""
        for (i in picked) {
            makeComb += i
        }
        set.add(makeComb)
    }

    fun find(baseArr: IntArray, picked: IntArray, visited: Array<Boolean>, depth: Int, size: Int, num: Int) {
        if (depth == num) {
            addArr(picked)
            return
        } else {
            for (i in 0 until size) {
                if(!visited[i]) {
                    visited[i] = true
                    picked[depth] = baseArr[i]
                    find(baseArr, picked, visited, depth + 1, size, num)
                    visited[i] = false
                }
            }
        }
    }
    find(arr, picked, visited, 0, arr.size, num)
    return set.toTypedArray()
}
```

```
[
    123, 124, 132, 134, 
    142, 143, 213, 214,
    231, 234, 241, 243, 
    312, 314, 321, 324, 
    341, 342, 412, 413, 
    421, 423, 431, 432
]
```


- 입력 : IntArray, 출력 : Array<IntArray>

```java
fun main() {
    var arr = intArrayOf(1, 2, 3, 4)
    println(getPermutation(arr, 3).contentDeepToString())
}

fun getPermutation(arr: IntArray, num: Int): Array<IntArray> {
    var visited = Array(arr.size) { false }
    var picked = IntArray(arr.size-1) {0}
    var set = arrayListOf<IntArray>()

    fun addArr(picked: IntArray) {
        var makeComb = arrayListOf<Int>()
        for (i in picked) {
            makeComb.add(i)
        }
        set.add(makeComb.toIntArray())
    }

    fun find(baseArr: IntArray, picked: IntArray, visited: Array<Boolean>, depth: Int, size: Int, num: Int) {
        if (depth == num) {
            addArr(picked)
            return
        } else {
            for (i in 0 until size) {
                if(!visited[i]) {
                    visited[i] = true
                    picked[depth] = baseArr[i]
                    find(baseArr, picked, visited, depth + 1, size, num)
                    visited[i] = false
                }
            }
        }
    }
    find(arr, picked, visited, 0, arr.size, num)
    return set.toTypedArray()
}
```

```
[
    [1, 2, 3], [1, 2, 4], [1, 3, 2], [1, 3, 4], 
    [1, 4, 2], [1, 4, 3], [2, 1, 3], [2, 1, 4], 
    [2, 3, 1], [2, 3, 4], [2, 4, 1], [2, 4, 3], 
    [3, 1, 2], [3, 1, 4], [3, 2, 1], [3, 2, 4], 
    [3, 4, 1], [3, 4, 2], [4, 1, 2], [4, 1, 3], 
    [4, 2, 1], [4, 2, 3], [4, 3, 1], [4, 3, 2]
]
```
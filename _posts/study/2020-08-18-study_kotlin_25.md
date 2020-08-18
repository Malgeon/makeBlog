---
layout: post
author: study
title:  "Kotlin 객체편 - [15]"
description: "Kotlin 배열 - 1"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


배열 표현하기
- arrayOf()나[-헬퍼 함수] Array() 생성자를 사용해 배열을 생성한다
- arrayOfNulls() : 빈배열

```java
val numbers = arrayOf(4, 5, 7, 3) // 정수형으로 초기화된 배열
val animals = arrayOf("Cat", "Dog", "Lion") // 문자열형으로 초기화된 배열
...

for(element in numbers) { // 정수형으로 초기화된 배열 출력하기
    println(element)
}
```

다차원 배열
```java
val array1 = arrayOf(1, 2, 3)
val array2 = arrayOf(4, 5, 6)
val array3 = arrayOf(7, 8, 9)

val arr2d = arrayOf(array1, array2, array3)
```

```java
val arr2d = arrayOf(arrayOf(1, 2, 3), arrayOf(4, 5, 6), arrayOf(7, 8, 9))
```

```java
for (e1 in arr2d) {
    for (e2 in e1) {
        print(e2)
    }
    println()
}
```

다양한 자료형
```java
val mixArr = arrayOf(4, 5, 6, 1, "Chike", false) // 정수, 문자열, Boolean 혼합
```
다양한 자료형 혼합 가능

특정 자료형으로 제한하고자 할 경우 
- arrayOf<자료형>() 
- 자료형 이름 + ArrayOf()
    - charArrayOf(), booleanArrayOf(), longArrayOf(), shortArrayOf(), byteArrayOf(), intArrayOf()
    - ubyteArrayOf(), ushortArrayOf(), uintArrayOf(), ulongArray()


배열 요소에 접근

선언부
```java
// 코틀린 표준 라이브러리의 Array.kt
public class Array<T> {
    public inline constructor(size: Int, init: (int) -> T)
    public operator fun get(index: Int): T
    public operator fun set(index: Int, value: T): Unit
    public val size: Int
    public operator fun iterator(): Iterator<T>
}
```

연산자를 통한 접근
```java
arr.get(index) -> value = arr[index]
arr.set(index) -> arr[index] = value
```

예시
- 읽기 접근
```java
val arr = intArrayOf(1, 2, 3, 4, 5)
println(arr.get(2)) // 게터를 통한 접근
println(arr[2]) // 연산자 오버로딩으로 대괄호를 통한 접근

```
```java
val arr2d = arrayOf(arrayOf(1, 2, 3), arrayOf(4, 5, 6), arrayOf(7, 8, 9))
println(arr2d[2][1]) // 8을 출력
```
- 쓰기 접근
```java
arr.set(2, 7) // 인덱스 2번 요소를 값 7로 교체
arr[0] = 8 // 인덱스 0번 요소를 값 8로 교체
arr2d[2][1] = 2 // 다차원 배열의 요소를 교체
println("size: ${arr.size} arr[0]: ${arr[0]}, arr[2]: ${arr[2]}")
```

practice
```java
import java.util.Arrays // Arrays를 사용하기 위해

fun main() {
    val arr = arrayOf(1, 2, 3, 4, 5)
    println(arr.get(2)) // 3
    println(arr[2]) // 3

    println(arr.size) // 5

    for (ele in arr) {
        print(ele) // 12345
    }
    println()
    println(Arrays.toString(arr)) // [1, 2, 3, 4, 5]
    println(arr.sum()) // 15

    arr.set(1, 8)
    println(Arrays.toString(arr)) // [1, 8, 3, 4, 5]
    arr[1] = 7
    println(Arrays.toString(arr)) // [1, 7, 3, 4, 5]
}
```
```java
import java.util.Arrays 

fun main(args: Array<String>) {
    val arr = intArrayOf(1, 2, 3, 4, 5)

    println("arr: ${Arrays.toString(arr)}") // Arrays.toString()은 배열의 내용을 문자열로 변환함
    println("size: ${arr.size}") // size는 배열의 크기를 나타냄
    println("sum: ${arr.sum()}") // sum() 메서드는 배열의 합을 계산해 줌

    // 게터에 의한 접근과 대괄호 연산자 표깁ㅂ
    println(arr.get(2))
    println(arr[2])

    // 세터에 의한 값의 설정
    arr.set(2, 7)
    arr[0] = 8
    println("size: ${arr.size} arr[0]: ${arr[0]}, arr[2]: ${arr[2]}")

    // 루프를 통한 배열 요소의 접근
    for (in in 0..arr.size - 1) {
        println("arr[$i] = ${arr[i]}")
    }
}
```

배열 출력을 도와주는 함수
toString
```java
val arr = intArrayOf(1, 2, 3, 4, 5)
println(Arrays.toString(arr)) // [1, 2, 3, 4, 5]
```

deepToString() [다차원 배열에서 사용!]
```java
val array = arrayOf(intArrayOf(1, 2),
            intArrayOf(3, 4)
            intArrayOf(5, 6, 7))

println(Arrays.deepToString(array)) // [[1, 2], [3, 4], [5, 6, 7]]
```
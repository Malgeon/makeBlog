---
layout: post
author: study
title:  "Kotlin 객체편 - [16]"
description: "Kotlin 배열 - 2"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 배열의 생성

표현식을 통한 생성
```
val|var 변수명 = Array(요소 개수, 초기값)
```
- 초기값으로 람다식 함수 init: (Int) -> T와 같이 정의
- 예시) 2씩 곱해지는 정수의 짝수 5개(0, 2, 4, 6, 8)의 요소

```java
val arr3 = Array(5, {i -> i * 2})
println("arr3: ${Arrays.toString(arr3)}")
```

많은 양의 배열 생성
```java
var a = arrayOfNulls <Int> (1000) // 1000개의 null로 채워진 정수 배열
```

```java
var a = Array(1000, { 0 }) // 0으로 채워진 배열
```

특정 클래스 객체로 배열 생성
```java
var a = Array(1000, { i -> myClass(i) })
```


배열에 요소 추가하고 잘라내기
배열이 일단 정의되면 고정됨
- 새로 할당하는 방법으로 추가하거나 잘라낼 수 있음
```java
val arr1 = int ArrayOf(1, 2, 3, 4, 5) // 다섯개로 고정된 배열(고정된다!)

// 하나의 요소를 추가한 새 배열 생성
val arr2 = arr1.plus(6)
println("arr2: ${Arrays.toString(arr2)}")
```

```java
// 필요한 범위를 잘라내 새 배열 생성
val arr3 = arr1.sliceArray(0..2) // 인자에 잘라낼 인덱스의 범위를 지정
println("arr3: ${Arrays.toString(arr3)}")
```

기타 배열 관련 API
API 사용 예
```java
// 첫 번째와 마지막 요소 확인
println(arr.first())
println(arr.last())

// 요소 3의 인덱스 출력
println("indexOf(3): ${arr.indexOf(3)}")

// 배열의 평균 값 출력
println("average: ${arr.average()}")

// count에 의한 요소 개수
println("count: ${arr.count()}")
```

존재 여부 확인
```java
operator fun <T> Array<out T>.contains(element: T): Boolean
```

```java
println(arr.contains(4)) // arr 배열에 요소 4가 포함되었는지 확인하고 true 반환
println(4 in arr)
```

```java
fun main() {
    val b = Array(10, { 0 })
    // b[0] = "Hello World" // Error! 이미 정수형으로 초기화 되었으므로 
    // b[1] = 1.1 // Error! Float도 안된다.
    println(Arrays.toString(b)) // [ 0, ..., 0]

    val c = Array<Any>(10, { 0 })
    c[0] = "Hello World"
    c[1] = 1.1

    val d = Array<Number>(10, { 0 })
    //d[0] = "Hello World" // Error! Number형의 자료형으로 제한
    d[1] = 1.1

}
```


### 배열의 순환

#### 순환 메서드의 사용
```java
// forEach에 의한 요소 순환
arr.forEach { element -> print("$element ") }
arr.forEach { print(it) }

// forEachIndexed에 의한 요소 순환
arr.forEachIndexed({i, e -> println("arr[$i] = $e")})
```

forEachindexed
- 인덱스는 i로, 요소는 e로 받아 화살표 표현식 오른쪽의 구문처리

#### Iterator의 이용
```java
// Iterator를 사용한 요소 순환
val iter: Iterator<Int> = arr.iterator()
while(iter.hasNext()) {
    val e = iter.next()
    print("$e ")
}
```

순환 방법에 따른 속도차이
- 정수형을 사용할 경우는 for 문이 빠르다.
- List 와 같은 Collection을 사용할 경우 순환 메서드가 빠르다.
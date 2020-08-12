---
layout: post
author: study
title:  "Kotlin 객체편 - [12]"
description: "Kotlin 제네릭 함수 혹은 메서드"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


## 제네릭 함수 혹은 메서드
- 해당 함수나 메서드 앞쪽에 <T>와 같이 지정
- 자료형의 결정은 함수가 호출될 때 컴파일러가 자료형 추론
- 이 자료형은 반환 자료형과 매개변수 자료형에 사용할 수 있다.

```
fun <형식 매개변수[, ...]> 함수명(매개변수: <매개변수 자료형>[, ...]): <반환 자료형>
```

```javascript
fun <T> genericFunc(arg: T): T? { ... } // 매개변수와 반환 자료형에 형식 매개변수 T가 사용된다.

fun <K, V> put(key: K, value: V): Unit { ... } // 형식 매개변수가 여러 개인 경우
```

예시
```javascript
fun <T> find(a: Array<T>, Target: T): Int {
    for (i in a.indices) {
        if (a[i] == Target) return i
    }
    return -1
}

fun main() {
    val arr1: Array<String> = arrayOf("Apple", "Banana", "Cherry", "Durian")
    val arr2: Array<Int> = arrayOf(1, 2, 3, 4)

    println("arr.indices ${arr1.indices}") // indices는 배열의 유효 범위 반환  return => arr.indices 0..3
    println(find<String>(arr1, "Cherry")) // 요소 C의 인덱스 찾아내기  return => 2
    println(find(arr2, 2)) // 요소의 인덱스 찾아내기 return => 1
}
```


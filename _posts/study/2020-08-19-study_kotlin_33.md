---
layout: post
author: study
title:  "Kotlin 객체편 - [23]"
description: "확장함수 - [1]"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 컬렉션의 확장 함수
코틀린은 컬렉션을 위한 많은 확장 함수를 제공하고 있다.
확장 함수 범주
- 연산자(operators) 기능의 메서드: 더하고 빼는 등의 기능
- 집계(aggregators) 기능의 메서드: 최대, 최소, 집합, 총합 등의 계산 기능
- 검사(checks) 기능의 메서드: 요소를 검사하고 순환하기 위한 기능
- 필터(filtering) 기능의 메서드: 원하는 요소를 골라내기 위한 기능
- 변환(transformers) 기능의 메서드: 뒤집기, 정렬, 자르기 등의 변환 기능


예시
```java
fun main() {
    // 연산자를 사용하게 될 경우, 기존 컬렉션의 값을 바꾸는 것이 아닌 새로운 컬렉션을 만드는 것임을 기억하자!
    val list1 = listOf("one", "two", "three")
    val list2 = listOf(1, 2, 3)
    val list3: List<Int> = listOf(1, 2, 3)
    // val list3: List<Int> = listOf(1, 2, 3, "Hello") // Error!

    println(list1 + "four") // + 연산자를 사용한 문자열 요소 추가 : [one, two, three, four]
    println(list1 - "one") // 요소의 제거 : [two, three]
    println(list1) // [one, two, three] 불변형!
    println(list1 + listOf("abc", "def")) // 두 lIst의 병합 : [one, two, three, abc, def]
    
    println(list2 + 4) // [1, 2, 3, 4]
    println(list2 + "Hello") // [1, 2, 3, Hello] 
    println(list2 - 1) // [2, 3]
    println(list2 - listOf(2, 3, 4)) // 일차하는 요소의 제거 : [1]
    println(list2) // [1, 2, 3]
    
    println(list3 + "Hello") // [1, 2, 3, Hello] 새로운 컬렉션이 생성된다고 할수 있기 때문에 가능하다.

    val map1 = mapOf("hi" to 1, "hello" to 2, "Goodbye" to 3)
    println(map1 + Pair("Bye", 4)) // Pair()를 사용한 Map의 요소 추가
    println(map1 - "hello") // 일치하는 값의 제거
    println(map1 + mapOf("Apple" to 4, "Orange" to 5)) // Map의 병합
    println(map1 - listOf("hi", "hello")) // List에 일치하는 값을 Map에서 제거
}
```


### 요소의 처리와 집계에 대한 연산
요소를 집계를 위한 확장 함수
- forEach, forEachIndexed, onEach, count, max, min, maxBy, minBy, fold, reduce, sumBy 등

요소의 순환
- forEach: 각 요소를 람다식으로 처리한 후 컬렉션을 반환하지 않음
- onEach: 각 요소를 람다식으로 처리한 후 컬렉션을 반환 받음

forEach 예시
```java
fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6)
    val listPair = listOf(Pair("A", 300), Pair("B", 200), Pair("C", 100))
    val map = mapOf(11 to "Java", 22 to "Kotlin", 33 to "C++")

    // forEach: 각 요소를 람다식으로 처리
    list.forEach { print("$it ")}
    println()
    list.foreachInexed { index, value -> println("index[$index] : $value") } // 인덱스 포함
}
```
```
1 2 3 4 5 6
index[0] : 1 
index[1] : 2
index[2] : 3
index[3] : 4
index[4] : 5
index[5] : 6
```

onEach 예시
```java
    // onEach: 각 요소를 람다식으로 처리한 후 컬렉션으로 반환
    val returnedList = list.onEach { print(it) }
    println()
    val returnedMap = map.onEach { println("key: ${it.ket}, value: ${it.value}") }
    println("returnedList = $returnedList")
    println("returnedMap = $returnedMap")
```
```
123456
key: 11, value: Java
key: 22, value: Kotlin
key: 33, value: C++
returnedList = [1, 2, 3, 4, 5, 6]
returnedMap = {11=Java, 22=Kotlin, 33=C++}
```


### 요소를 집계를 위한 확장 함수
각 요소의 정해진 식 적용
- fold와 reduce : 초기 값과 정해진 식에 따라 요소에 처리하기 위해 
    - fold는 초기값과 정해진 식에 따라 처음 요소부터 끝 요소에 적용해 값 반환
    - reduce는 fold와 동일하지만 초기값을 사용하지 않는다.

- foldRight, reduceRight: 위의 개념과 동일하지만 요소의 마지막(오른쪽)요소에서 처음 요소로 순서대로 적용한다.



```java
val list = listOf(1, 2, 3, 4, 5, 6)

// fold: 초기값과 정해진 식에 따라 처음 요소부터 끝 요소에 적용하며 값을 생성
println(list.fold(4) { total, next -> total + next }) // 4 + 1 + ... + 6 = 25
println(list.fold(1) { total, next -> total * next }) // 1 * 1 * 2 * ... * 6 = 720

// foldRight: fold와 같고 마지막 요소에서 처음 요소로 반대로 적용
println(list.foldRight(4) { total, next -> total + next }) // 4 + 6 + ... + 1 = 25
println(list.foldRight(1) { total, next -> total * next }) // 1 * 6 * ... * 1 = 720

// reduce: fold와 동일하지만 초기값을 사용하지 않음
println(list.reduce { total, next -> total + next }) // 1 + ... + 6 = 21
println(list.reduceRight) { total, next -> total + next }) // 6 + ... + 1 = 21
```
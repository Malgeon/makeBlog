---
layout: post
author: study
title:  "Kotlin 객체편 - [20]"
description: "Kotlin 컬렉션 - List - [2]"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


### 가변형 List - arrayListOf()
arrayListOf()
- 가변형 헬퍼 함수를 사용하면 손쉽게 요소를 추가하거나 삭제할 수 있따.
- arrayListOf()는 가변형 List를 생성하지만 반환자료형은 자바의 ArrayList

```java
public fun <T> arrayListOf(vararg elements: T): ArrayList<T>
```


```java
import java.util.*

fun main() {
    // 가변형 List를 생성하고 자바의 ArrayList로 반환
    val stringList: ArrayList<String> = arrayListOf<String>("Hello", "Kotlin", "Wow")
    stringList.add("Java") // 추가
    stringList.remove("Hello") // 삭제
    println(stringList)
}
```
```
[Kotlin, wow, Java]
```


### MutableList 인터페이스를 이용하는 헬퍼 함수
가변형 mutableListOf() 함수
- 요소의 추가, 삭제 또는 교체를 위해 mutablelistOf()를 통해 생성
- MutableList 형으로 반환
```
public fun <T> mutableListOf(varage elements: T): MutableList<T>
```

add()와 removeAt() 메서드를 통해 추가/삭제가 가능하다
```java
fun main() {
    // 가변형 List의 생성 및 추가, 삭제, 변ㄱㅇ
    val mutableList: MutableList<String> = mutableListOf<String>("Kildong", "Dooly", "Chelsu")
    mutableList.add("Ben") // 추가
    mutableList.removeAt(1) // 인덱스 1 삭제
    mutableList[0] = "Sean" // 인덱스 0을 변경, set(index: Int, element E)과 같은 역할
    println(mutableList)

    // 자료형의 혼합
    val mutableListMixed = mutableListOf("Android", "Apple", 5, 6, 'X')
}
```
```
[Sean, Chelsu, Ben]
[Android, Apple, 5, 6, X]
```

toMutableList()
```java
fun main{
    val name: List<String> = listOf("one", "two", "three") // 불변형 List 초기화

    val mutableNames = names.toMutableList() // 새로운 가변형 List가 만들어짐
    mutableNames.add("four") // 가변형 List에 하나의 요소 추가
    println(mutableNames)
}
```
```
[onem two, three, four]
```


### List와 배열의 차이
- Array 클래스에 의해 생성되는 배열 객체는 내부 구조상 고정된 크기를 가진다.
- 코틀린의 List<T>와 MutableList<T>는 인터페이스로 설계되어 있고 이것을 하위에서 특정한 자료구조로 구현
    - LinkedList<T>, ArrayList<T>
```java
val list1: List<Int> = LinkedList<Int>()
val list2: List<Int> = ArrayList<Int>()
```
    - 고정된 크기의 메모리가 아니기 때문에 자료구조에 따라 늘리거나 줄이는 것이 가능
- Array<T>는 제네릭 관점에서 상/하위 자료형 관계가 성립하지 않는 무변성이다.
- List<T>는 공변성이기 때문에 하위인 List<Int>가 List<Number>에
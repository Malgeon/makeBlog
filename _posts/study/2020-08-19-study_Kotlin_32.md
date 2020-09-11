---
layout: post
author: study
title:  "Kotlin 객체편 - [22]"
description: "Collection.Map"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### Map

개념 
- 키(Key)와 값(Value)으로 구성된 요소를 저장한다.
    - 키와 값은 모두 객체
- 키는 중복될 수 없지만 값은 중복 저장될 수 있다.
    - 만약 기존에 저장된 키왇 ㅗㅇ일하면 기존의 값은 없어지고 새로운 값으로 대체 된다.

Map의 생성
- 불변형: mapOf()
- 가변형: mutableMapOf()


### mapOf() - 불변형

```
val map: Map<키자료형, 값자료형> = mapOf(키 to 값[, ...])
```

키와 쌍은 '키 to 값' 형태로 나타낸다.
```java
fun main() {
    // 불변형 Map의 선언 및 초기화
    val langMap: Map<Int, String> = mapOf(11 to "Java", 22 to "Kotlin", 33 to "C++")

    for ((key, value) in langMap) {
        println("key = $key, value = &value")
    }

    println("langMap[22] = ${langMap[22]}") // 키 22에 대한 요소 출력
    println("langMap.get(22) = ${langMap.get(22)}") // 위와 동일한 표현
    println("langMap.keys = ${langMap.keys}") // 맵의 모든 키 출력
}
```

Map의 멤버

| 멤버 | 설명 |
| --- | --- |
| size | 맵 컬렉션의 크기를 반환한다. |
| keys | 이 프로퍼티는 Set의 모든 키를 반환한다. |
| values | 이 프로퍼티는 Set의 모든 값을 반환한다. |
| isEmpty() | 맵이 비어있는지 확인하고 비어 있으면 true를, 아니면 false를 반환한다. |
| containsKey(key: K) | 인자에 해당하는 키가 있다면 true를, 아니면 false를 반환한다. |
| containsValue(value: V) | 인자에 해당하는 값이 있다면 true를, 아니면 false를 반환한다. |
| get(key: K) | 키에 해당하는 값을 반환하며, 없으면 null을 반환한다. |


MutableMap의 추가 멤버
- 추가, 삭제가 가능한 메서드 제공

| 멤버 | 설명 |
| --- | --- |
| put(key: K, value: V) | 키와 값의 쌍을 맵에 추가한다. |
| remove(key: K) | 키에 해당하는 요소를 맵에서 제거한다. |
| putAll(from: Map<out K, V>) | 인자로 주어진 Map 데이터를 갱신하거나 추가한다. |
| clear() | 모든 요소를 지운다. |


mutableMapOf() 예시
```java
fun main() {
    // 가변형 Map의 선언 및 초기화
    val capitalCityMap: MutalbleMap<String, String> // 선언 시 키와 값의 자료형을 명시할 수 있음(추론 가능 시 생략도 가능)
        = mutableMapOf("Korea" to "Seoul", "China" to "Beijing", "Japan" to "Tokyo")
    println(capitalCityMap.values) // 값만 출력
    println(capitalCityMap.keys) // 키만 출력
    capitalCityMap.put("UK", "London") // 요소의 추가
    capitalCityMap.remove("China") // 요소의 삭제
    println(capitalCityMap)
}
```
```
[Seoul, Beiging, Tokyo]
[Korea, China, Japan]
{Korea=Seoul, Japan=Tokyo, UK=London}
```

putAll()을 사용해 맵 객체 통합해보기
- 기존 요소에 추가된 요소를 병합 할 수 있다.

```java
// putAll()을 사용한 맵의 추가
val addData = mutableMapOf("USA" to "Washington")
capitalCityMap.putAll(addData)
println(capitalCityMap)
```
- 기존 요소

```
{Korea=Seoul, Japan=Tokyo, UK=London}
```

- 병합된 요소

```
{Korea=Seoul, Japan=Tokyo, UK=London, USA=Washington}
```


### Map을 위한 기타 자료구조

특정 자료구조 이용
- Map에서도 자바의 HashMap, SortedMap과 LinkedHashMap을 사용할 수 있다.
- hashMapOf(), sortedMapOf(), linkedMapOf()로 각각 초기화
- SortedMap의 경우 기복적으로 키에 대해 오름차순 정렬된 형태로 사용
- 내부 구조는 앞서 설명했던 Set의 구조와 비슷하게 해시, 트리, 링크드 리스트의 자료구조로 구현


예시
```java
import java.util.*

fun main() {
    // java.util.HashMap의 사용
    val hashMap: HashMap<Int, String> = hashMapOf(1 to "Hello", 2 to "World")
    println("hashMap = $hashMap")

    // java.util.SortedMap의 사용
    val sortedMap: SortedMap<Int, String> = sortedMapOf(1 to "Apple", 2 to "Bababa")
    println("sortedMap = $sortedMap")

    // java.util.LinkedHashMap의 사용
    val linkedHash: LinkedHashMap<Int, String> = linkedMapOf(1 to "Computer", 2 to "Mouse")
    println("linkedHash = $linkedHash")
}
```
```
hashMap = {1=Hello, 2=World}
sortedMap = {1=Apple, 2=Banana}
linkedHash = {1=Computer, 2=Mouse}
```
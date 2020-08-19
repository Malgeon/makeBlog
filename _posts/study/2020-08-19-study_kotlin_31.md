---
layout: post
author: study
title:  "Kotlin 객체편 - [21]"
description: "Kotlin 컬렉션 - Set"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### Set

개념 
- 정해진 순서가 없는 요소들의 집합(set)
- 집합의 개념이기 때문에 동일한 요소를 중복해서 가질 수 없다.

생성 헬퍼 함수
- 불변형 Set : setOf()
- 가변형 Set : mutableSetOf()


예시 - 불변형
```java
fun main() {
    val mixedTypesSet = setOf("Hello", 5, "world", 3.14, 'c') // 자료형 혼합 초기화
    var intSet: Set<Int> = setOf<Int>(1, 5, 5) // 정수형만 초기화

    println(mixedTypeSet)
    println(intSet)
}
```
```
[Hello, 5, world, 3.14, c]
[1, 5]
```
- intSet에서는 중복된 요소인 5가 결과에서 하나만 나타남

예시 - 가변형
```java
fun main() {
    // 가변형 Set 정의하기
    val animals = mutableSetOf("Lion", "Dog", "Cat", "Python", "Hippo")
    println(animals)
    // 요소의 추가
    animals.add("Dog") // 이미 존재하므로 변화 없음
    println(animals)
    // 요소의 삭제
    animals.remove("Phthon")
    println(animals)
}
```
```
[Lion, Dog, Cat, Python, Hippo]
[Lion, Dog, Cat, Python, Hippo]
[Lion, Dog, Cat, Hippo]
```

### Set의 여러 가지 자료구조

hashSetOf()
- 이 헬퍼 함수를 통해 생성하면 해시 테이블에 요소를 저장할 수 있는 자바의 HashSet컬렉션을 만든다.
- HashSet은 불변성 선언이 없기 때문에 추가 및 삭제 등의 기능을 수행할 수 있다.

해시 테이블(Hash Table)
- 해시 테이블이란 내부적으로 키와 인덱스를 이용해 검색과 변경 등을 매우 빠르게 처리할 수 있는 자료구조

HashSet의 생성
```java
fun main() {
    val intsHashSet: HashSet<Int> = hashSetOf(6, 3, 4, 7) // 불변성 기능이 없음
    intsHashSet.add(5) // 추가
    intsHashSet.remove(6) // 삭제
    println(intsHashSet)
}
```
```
[3, 4, 5, 7]
```
- HashSet은 결과와 같이 입력 순서와 중복된 요소는 무시된다.
- 정렬 기능은 없지만 해시값을 통해 요소를 찾아내므로 검색 속도는 O(1)의 상수 시간
    - 필요한 값을 요청과 즉시 바로 찾아냄


### 자바의 TreeSet 컬렉션

sortedSetOf()
- 자바의 TreeSet 컬렉션을 정렬된 상태로 반환
- java.util.* 패키지를 import 해야 한다.
- TreeSet은 저장된 데이터의 값에 따라 정렬
    - 이진 탐색 트리(binary-search tree)인 RB(red-black) 트리 알고리즘을 사용
- HashSet보다 성능이 조금 떨어지고 데이터를 추가하거나 삭제하는 데 시간이 걸리지만 검색과 정렬이 뛰어나다는 장점이 있다.


이진 탐색 트리와 RB 트리
- 이진 탐색 트리가 한쪽으로 치우친 트리 구조를 가지게 되는 경우 트리 높이만큼 시간이 걸리게 되는 최악의 경우 시간이 만들어 질 수 있다.
- RB 트리는 이 단점을 Red와 Black의 색상으로 치우친 결과 없이 구분되도록 해서 최악의 경우에도 검색 등의 처리에서 일정 시간을 보장하는 자료구조

sortedSetOf()를 사용한 Set의 생성
```java
import java.util.*

fun main() {
    // 자바의 java.util.TreeSet 선언 즉, 자바는 불변형이 없으므로 모두 가변형!
    val intsSortedSet: TreeSet<Int> = sortedSetOf(4, 1, 7, 2)
    intsSortedSet.add(6)
    intsSortedSet.remove(1)
    println("intsSortedSet = ${intsSortedSet}")
    intsSortedSet.clear() // 모든 요소 삭제
    println("intsSortedSet = ${intsSortedSet}")
}
```
```
intsSortedSet = [2, 4, 6, 7]
intsSortedSet = []
```

### 자바의 LinkedHashSet
linkedSetOf() 함수
- 자바의 LinkedHashSet 자료형을 반환하는 헬퍼 함수
- 자료구조 중 하나인 링크드 리스트(Linked-list)를 사용해 구현된 해시 테이블에 요소를 저장
- HashSet, TreeSet보다 느리지만 데이터 구조상 다음 데이터를 가리키는 포인터 연결을 통해 메모리 저장 공간을 조금 더 효율적으로 사용한다.
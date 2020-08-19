---
layout: post
author: study
title:  "Kotlin 객체편 - [19]"
description: "Kotlin 컬렉션 & 컬렉션 List - [1]"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


### Collection

- 자주 사용되는 기초적인 자료구조를 모아놓은 일종의 프레임워크로 표준 라이브러리로 제공된다.

코틀린의 컬렉션
- 컬렉션의 종류로는 List, Set, Map 등이 있으며 자바와는 다르게 불변형(immutable)과 가변형(mutable)으로 나뉘어 컬렉션을 다룰 수 있다.

| 컬렉션 | 불변형(읽기 전용) | 가변형 |
| --- | --- | --- |
| List | listOf | mutableListOf, arrayListOf |
| Set | setOf | mutableSetOf, hashSetOf, linkedSetOf, sortedSetOf |
| Map | mapOf | mutableMapOf, hashMapOf, linkedMapOf, sortedMapOf |


- 다이어그램의 가장 상위의 Iterable 인터페이스는 컬렉션이 연속적인 요소를 표현 할 수 있게 한다.

![collections]({{ site.baseurl }}/assets/images/study/kotlin/kotlin_oop_collections.JPG)


### Collection 인터페이스

Collection 인터페이스의 특징
- Iterable로부터 확장
- 불변형이므로 Collection으로부터 확장된 Set과 List는 읽기 전용의 컬렉션됨

Collection 인터페이스의 멤버
| 멤버 | 설명 |
| --- | --- |
| size | 컬렉션의 크기를 나타낸다 |
| isEmpty() | 컬렉션이 비어 있는 경우 true를 반환한다 |
| contains(element: E) | 특정 요소가 있다면 true를 반환한다 |
| containsAll(element: Collection<E>) | 인자로 받아들인 Collection이 있다면 true를 반환한다. |


### MutableIterable과 MutableCollection 인터페이스
특징
- 가변형 컬렉션을 지원하기 위해 준비된 인터페이스
- 요소를 추가하거나 제거하는 등의 기능을 수행할 수 있따.

MutableCollection의 멤버
| 멤버 | 설명 |
| --- | --- |
| add(element: E) | 인자로 전달반은 요소를 추가하고 true를 반환하며, 이미 요소가 있거나 중복이 허용되지 않으면 false를 반환한다. |
| remove(element: E) | 인자로 받은 요소를 삭제하고 true를 반환하며, 삭제하려는 요소가 없다면 false를 반환한다. |
| addAll(elements: Collection<E>) | 컬렉션을 인자로 전달받아 모든 요소를 추가하고 true를 반환하며, 실패 시 false를 반환한다. |
| removeAll(elements: Collection<E>) | 컬렉션을 인자로 전달받아 모든 요소를 삭제하고 true를 반환하며, 실패 시 false를 반환한다. |
| retainAll(elements: Collection<E>) | 인자로 전달받은 컬렉션의 요소만 보유한다. 성공 시 true를 반환하고 실패 시 false를 반환한다. |
| clear() | 컬렉션의 모든 요소를 삭제한다. |


### List
List
- 순서에 따라 정렬된 요소를 가지는 컬렉션 (가장 많이 사용되는 컬렉션 중에 하나)
- 값을 변경할 수 없는 불변형 List를 만들기 위해 헬퍼 함수인 listOf()를 사용
- 값을 변경할 수 있는 가변형을 표현하기 위해서는 mutablelistOf()를 사용
- 인자는 원하는 만큼의 가변 인자를 가지도록 vararg로 선언 가능

헬퍼 함수
- 객체 생성 시 요소를 직접 선언하기 보다 특정 함수의 도움을 통해 생성


### listOf()
```java
public fun <T> listOf(vararg elements: T): List<T>
```
- vararg는 가변 인자를 받을 수 있기 때문에 원하는 만큼 요소를 지정
- 값을 반환할 때는 List<T>를 사용
- 형식 매개변수 <T>는 필요에 따라 원하는 자료형을 지정해 선언
    - 사용하지 않으면 <Any>가 기본값이며 어떤 자료형이든 혼합 사용 가능

```java
fun main() {
    // 불변형 List의 사용
    var numbers: List<Int> = listOf(1, 2, 3, 4, 5)
    var names: List<String> = listOf("one", "two", "three")

    for (name in names) {
        println(name)
    }

    for (num in numbers) print(num) // 한 줄에서 처리하기
    println() // 내용이 없을 때는 한 줄내리는 개행
}
```

형식 매개변수 생략 예시
```java
var mixedTypes = listOf("Hello", 1, 2.445, 's')
/* 아래와 같은 표현이다
var mixedTypes: List<Any> = listOf("Hello", 1, 2.445, 's')
*/
```

### 컬렉션에 접근할 때
- for와 .indices 멤버를 통한 접근
```java
val fruits = listOf("apple", "banana", "kiwi")
// for과 in을 사용한 출력
for (item in fruits) {
    println(item)
}
```
```
apple
banana
kiwi
```

```java
for (index in fruits.indices) {
    println("fruits[$index] = ${fruits[index]}")
}
```
```
fruits[0] = apple
fruits[1] = banana
fruits[2] = kiwi
```


### 기타 List 생성 함수

emptyList()
- 빈 리스트를 생성
```java
val emptyList: List<String = emptyList<String>>()
```

listOfNotNull()
- null을 제외한 요소만 반환
```java
// null이 아닌 요소만 골라 컬렉션을 초기화
val nonNullsList: List<Int> = listOfNotNull(2, 45, 2, null, 5, null)
println(nonNullsList)
```
```
[2, 45, 2, 5]
```

### List 추가 멤버 메서드

| 멤버 메서드 | 설명 |
| --- | --- |
| get(index: Int) | 특정 인덱스를 인자로 받아 해당 요소를 반환한다. |
| indexOf(element: E) | 인자로 받은 요소가 첫 번째로 나타나는 인덱스를 반환하며, 없으면 -1을 반환한다. |
| lastIndexOf(element: E) | 인자로 받은 요소가 마지막으로 나타나는 인덱스를 반환하고, 없으면 -1을 반환한다. |
| listIterator() | 목록에 있는 iterator를 반환한다. |
|  subList(fromIndex: Int, toIndex: Int| 특정 인덱스의 from과 to 범위에 있는 요소 목록을 반환한다. |

practice
```java
// 자료형 확인 단축기 Ctrl + Shift
fun main() {
    var numbers1 = listOf(1, 2, 3, 4, 5)
    var numbers2: List<Int> = listOf(1, 2, 3, 4, 5)
    var names1 = listOf("one", "two", "three")
    var names2: List<String> = listOf("one", "two", "three")
    var mixed1 = listOf("one", 1, 1.5, 'c')
    var mixed2: List<Any> = listOf("one", 1, 1.5, 'c')

    println("numbers $numbers1") // [1, 2, 3, 4, 5]
    println("names $names1") // [one, two, three]
    println("mixed $mixed1") // [one, 1, 1.5, c]

    println(numbers1.size) // 5
    println(numbers1.indexOf(3)) // 2
    println(numbers1.get(0)) // 1
    println(numbers1[0]) // 1
    println(numbers1.contains(1)) // 
}
```
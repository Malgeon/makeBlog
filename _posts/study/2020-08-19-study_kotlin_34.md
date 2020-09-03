---
layout: post
author: study
title:  "Kotlin 객체편 - [24]"
description: "확장함수 - [2] (map, groupBy, sequence"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### Mapping 관련 연산

map()
- 주어진 컬렉션의 요소를 일괄적으로 모든 요소에 식을 적용해 새로운 컬렉션을 만든다.
- forEach와는 다르게 주어진 컬렉션을 건드리지 않는다.

```java
fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6)
    val listWithNull = listOf(1, null, 3, null, 5, 6)

    // map: 컬렉션에 주어진 식을 적용해 새로운 컬렉션 반환
    println(list.map { it * 2}) // [2, 4, 6, 8, 10, 12]

    // mapIndexed: 컬렉션에 인덱스를 포함해 주어진 식을 적용해 새로운 컬랙션 반환
    val mapIndexed = list.mapIndexed { index, it -> index * it }
    println(mapIndexed) // [0, 2, 6, 12, 20, 30]

    // mapNotNull: null을 제외하고 식을 적용해 새로운 컬렉션 반환
    println(listWithNull.mapNotNull { it?.times(2) }) // [2, 6, 10, 12]
}
```


flatMap()
- 각 요소에 식을 적용한 후 이것을 다시 하나로 합쳐 새로운 컬렉션을 반환

```java
// flatMap: 각 요소에 식을 적용 후 다시 합쳐 새로운 컬렉션을 반환
println(list.flatMap { listOf(it, 'A')})
val result = listOf("abc", "12").flatMap { it.toList() }
println(result)
```
```
[1, A, 2, A, 3, A, 4, A, 5, A, 6, A]
[a, b, c, 1, 2]
```

groupBy()
- 주어진 식에 따라 요소를 그룹화 하고 이것을 다시 Map으로 반환

```java
// groupBy: 주어진 함수의 결과에 따라 그룹하 하여 map으로 반환
val grpMap = list.groupBy { if (it % 2 == 0) "even" else "odd" }
println(grpMap) // {odd=[1, 3, 5], even=[2, 4, 6]}
```


### 요소 처리와 검색 연산

element 관련 연산
- 보통 인덱스와 함께 해당 요소의 값을 반환
- 식에 따라 처리하거나 인덱스를 벗어나면 null을 반환

```java
fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6)
    val listPair = listOf(Pair("A", 300), Pair("B", 200), Pair("C", 100), Pair("D", 200))
    val listRepeated = listOf(2, 2, 3, 4, 5, 5, 6)

    // elementAt: 인덱스에 해당하는 요소 반환
    println("elementAt: " + list.elementAt(1))

    // elementAtOrElse: 인덱스를 벗어나는 경우 식에 따라 결과 반환 아니면 요소 반환
    println("elementAtOrElse: " + list.elementAtOrElse(10, { 2 * it }))
    /*
    elementAtOrElse(10) { 2 * it } 표현식과 동일
    */

    // elementAtorNull: 인덱스를 벗어나는 경우 null 반환
    println("elementAtOrNull: " + list.elementAtOrNull(10))
}
```
```
elementAt: 2
elementAtOrElse: 20
elementAtOrNull: null
```


### 순서와 정렬 연산

컬렉션에 대한 정렬 연산
- reversed()
- sorted() / sortedDescending()
- sortedBy { it % 3 } / sortedByDescending { it % 3 }




### 시퀀스

sequence
- 순차적으로 요소의 크기를 특정하지 않고 추후에 결정하는 특수한 컬렉션
- 예를 들어 특정 파일에서 줄 단위로 읽어서 요소를 만들 때 시퀀스는 처리 중에는 계산하고 있지 않다가 toList()나 count()같은 최종 연산에 의해 결정


generateSequence
- 시드(seed) 인수에 의해 시작 요소의 ㄱ밧이 결정
```java
fun main() {
    // 시드값 1을 시작으로 1씩 증가하는 시퀀스 정의
    val nums: Sequence<Int> = generateSequence(1) { it + 1 }

    // take()를 사용해 원하는 요소 개수만큼 획득하고
    // toList()를 사용해 List 컬렉션으로 반환

    println(nums.take(10).toList())
}
```
```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```


map이나 fold같은 연산을 같이 이용
```java
val squares = generateSequence(1) {it + 1}.map {it * it}
println(squares.take(10).toList())

val oddSquares = squares.filter { it % 2 != 0 }
println(oddSquares.take(5).toList())
```
```
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
[1, 9, 25, 49, 81]
```

- 메서드 체이닝(chaining)하여 연속해서 쓴다면 하나의 구문이 끝날 때마다 중간 결과로 새로운 List를 계속해서 만들어 낸다.


asSequence()
- 중간 연산 결과 없이 한 번에 끝까지 연산한 후 결과를 반환
    - filter나 map을 메서드 체이닝해서 사용할 경우 순차적 연산이기 때문에 시간이 많이 걸릴 수 있지만 asSequence()를 사용하면 병렬 처리되기 떄문에 처리 성능이 좋아짐
```java
fun main() {
    // 단순 메서드 체이닝을 사용할 때
    val list1 = listOf(1, 2, 3, 4, 5)
    val listDefault = list1
            .map { println("map($it) "); it * it }
            .filter { println("filter($it) "); it % 2 == 0 }
    println(listDefault)
}
```
```
map(1)
map(2)
map(3)
map(4)
map(5)
filter(1)
filter(4)
filter(9)
filter(16)
filter(25)
[4, 16]
```

```java
...
val listSeq = ist1.asSequence()
        .map { print("map($it) "); it * it } // 1
        .filter { println("filter($it) "); it % 2 == 0 } // 2
        .toList() // 3
println(listSeq)
```
```
map(1) filter(1)
map(2) filter(4)
map(3) filter(9)
map(4) filter(16)
map(5) filter(25)
[4, 16]
```

- 3번 과정에 의해 최종 결과를 List 목록으로 변환할 때, 모든 연산이 수행되고 결과물이 새로운 리스트인 listSeq에 지정
- 요소의 개수가 많을 때 속도나 메모리 측면에서 훨씬 좋은 성능을 낼 수 있다
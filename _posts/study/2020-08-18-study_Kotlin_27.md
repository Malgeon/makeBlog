---
layout: post
author: study
title:  "Kotlin 객체편 - [17]"
description: "배열 정렬"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 배열의 정렬

정렬(Sort)
- 오름차순(ascending)으로 정렬하거나 내림차순(descending) 정렬
- Array는 기본적인 정렬 알고리즘을 제공한다.

정렬된 배열을 반환
- sortedArray()
- sortedArrayDescending()

원본 배열에 대한 정렬을 진행
- sort()
- sortDescending()


```java
fun main() {
    val arr = arrayOf(8, 4, 3, 2, 5, 9, 1)

    val sortedArr = arr.sortedArray()
    println(Array.toString(sortedArr)) // [1, 2, 3, 4, 5, 8, 9]

    val sortedArrDesc = arr.sortedArrayDescending()
    println(Arrays.toString(sortedArrDesc)) // [9, 8, 5, 4, 3, 2, 1]

    arr.sort(1, 3)
    println(Arrays.toString(arr))  // [8, .. 3, 4, .. 2, 5, 9, 1]
    arr.sortDescending() // parameter 옵션이 없다.
    println(Arrays.toString(arr))  // [9, 8, 5, 4, 3, 2, 1]

    // 3. List로 반환
    val listSorted: List<Int> = arr.sorted()
    val listDesc: List<Int> = arr.sortedDescending()

    println("LST: " + listSorted)
    println("LST: " + listDesc)

    // 4. SortBy를 이용한 특정 표현식에 따른 정렬
    val items = arrayOf<String>("Dog", "Cat", "Lion", "Kangaroo", "Po")
    items.sortBy { item -> item.length }
    println(Arrays.toString(items))
}
```

practice

```java
data class Product(val name: String, val price: DOuble)

fun main() {
    val products = arrayOf(
        Product("Snow Ball", 870.00),
        Product("Smart Phone", 999.00),
        Product("Drone", 240.00),
        Product("Mouse", 333.55),
        Product("Keyboard", 125.99),
        Product("Monitor", 1500.99),
        Product("Tablet", 512.99))

    products.sortBy { it.price }
    products.forEach { println(it) }
}
```
```java
data class Product(val name: String, val price: DOuble)

fun main() {
    val products = arrayOf(
        Product("Snow Ball", 870.00),
        Product("Smart Phone", 999.00),
        Product("Drone", 240.00),
        Product("Mouse", 333.55),
        Product("Keyboard", 125.99),
        Product("Monitor", 1500.99),
        Product("Tablet", 512.99))

    products.sortWith(
        Comparator<Product> { p1, p2 ->
            when {
                p1.price > p2.price -> 1
                p1.price == p2.price -> 0
                else -> -1
            }
        }
    )

    product.forEach{ println(it) }
    println()

    // CompareBy를 함께 사용해 두개의 정보 정렬
    // varargs(가변인수)로 받고 있으므로 두개 이상 사용 가능
    products.sortWith( compareBy( {it.name}, {it.price}))
    products.forEach { println(it) }
    println()

    // 지정된 필드의 가장 작은/큰 값 골라내기
    println(products.minBy { it.price })
    println(products.maxBy { it.price })
}
```

### -With, -By
- with : comparator를 이용하여 정렬을 할수 있게 해준다.

```java
fun <T> Array<out T>.sortedWith(
    comparator: Comparator<in T>
): List<T>
```
- By : 람다식을 이용해 정렬을 도와준다.

```java
inline fun <T, R : Comparable<R>> Array<out T>.sortedBy(
    crossinline selector: (T) -> R?
): List<T>
```

여기서 crossinlin selector이기 때문에 매개변수가 없다면 생략 가능

```java
val list = listOf("aa", "b", "bb", "a")
val sorted = list.sortedWith(compareBy { it.length })
println(sorted)
```
```
[b, a, aa, bb]
```

2개 이상의 람다식도 가능하다. 이때 해당 람다식은 이전 람다식이 같게되면 다음으로 진행하는 것으로 판단된다.

```java
val list = listOf("aa", "b", "bb", "a")
val sorted = list.sortedWith(compareBy(
    { it.length },
    { it }
))
println(sorted)
```
```
[a, b, aa, bb]
```


### 배열 필터링하기
filter() 메서드를 활용하면 원하는 데이터를 골라낼 수 있다.

```java
// 0 보다 큰 수 골라내기
val arr = arrayOf(1, -2, 3, 4, -5, 0)
arr.filter { e -> e > 0 }.forEach { e -> print("$e ")}
/* 같은 표현
arr.filter { it > 0 }.forEach { print(it + " ")}
*/
```

체이닝을 통한 필터링 예

```java
fun main() {
    val fruits = arrayOf("banana", "avocado", "apple", "kiwi")

    fruits
    .filter { it.startsWith("a") }
    .sortedBy { it }
    .map { it.toUpperCase() }
    .forEach { println(it) }
}
```

when 문을 사용한 요소 확인

```java
when {
    "apple" in fruits -> println("Apple!")
    ...
}
```

큰 값 작은 값 골라내기

```java
// 지정된 필드의 가장 작은/큰 값 골라내기
println(products.minBy { it.price })
println(products.maxBy { it.price })
```

배열 평탄화 하기
- flatten() : 다차원 배열을 단일 배열 생성

```java
fun main() {
    val numbers = arrayOf(1, 2, 3)
    val strs = arrayOf("one", "two", "three")
    val simpleArray = arrayOf(numbers, strs) // 2차원 배열

    simpleArray.forEach{ println(it) }

    val flattenSimpleArray = simpleArray.flatten() // 단일 배열로 변환하기
    println(flattenSimpleArray)
}
```


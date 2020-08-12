---
layout: post
author: study
title:  "Kotlin 객체편 - [11]"
description: "Kotlin 클래스 - 연산자 오버로딩"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


### 연산자 오버로딩(operator overloading)
- 연산자에 여러 가지 다른 의미의 작동을 부여
- 코틀린에서는 특정 연산자의 역할을 함수로 정의
- operator 키워드를 사용해 정의


### 수식 연산자
a + b 연산자 관련 메서드
```javascript
val a = 5
val b = 10
print(a.plus(b)) // print( a + b ) 와 동일
```

오버로딩된 plus() 함수의 종류
```javascript
// 표준 라이브러리의 Primitives.kt 파일의 일부
...
/// + operator for basic types
operator fun plus(other: Byte): Int
operator fun plus(other: Short): Int
operator fun plus(other: Int): Int
operator fun plus(other: Long): Long
operator fun plus(other: Float): Float
operator fun plus(other: Double): Double

// for string concatenation
operator fun String?.plus(other: Any?): String
...
```

오버로딩 예시
```javascript
class Point(val x: Int = 0, var y: Int = 10) {
    // plus 함수의 연산자 오버로딩
    operator fun plus(p: Point) : Point {
        return Point(x + p.x, y + p.y)
    }
}

fun main() {
    val p1 = Point(3, -8)
    val p2 = Point(2, 9)

    var point = Point()
    point = p1 + p2 // Point 객체의 +(operator plus!) 연산이 가능하게 됨
    println("point = (${point.x}, ${point.y})")
}
```

```javascript
// --연산의 오버로딩 추가
class Point(var x: Int = 0, var y: Int = 10) {
    ...
    operator fun dec() = Point(--x, --y)
}

fun main() {
    ...
    --point // --연산자
    println("point = (${point.x}, ${point.y})")
}
```

수식 연산자의 종류

| 표현식 | 함수 의미 |
| --- | --- |
| a + b | a.plus(b) |
| a - b | a.minus(b) |
| a * b | a.times(b) |
| a / b | a.div(b) |
| a % b | a.rem(b) (Kotlin 1.1부터, a.mod(b) (지원 중단) |
| a .. b | a.rangeTo(b) |


### 호출 연산자(invoke operator)
함수 호출을 돕는데 사용한다.
```javascript
class Manager {
    operator fun invoke(value: String) {
        println(value)
    }
}

fun main() {
    val manager = Manager()
    manager("Do something for me!") // manager.invoke("...") 형태로 호출되며 invoke가 생략 되었다. 
                                    // (invoke를 사용할때 해당 객체만 사용하면 생략 가능하다)
}
```
```javascript
val sum = { x: Int, y: Int -> x + y}
sum.invoke(3, 10) // 13
sum(3, 10) // 위와 동일한 결과
```


### 인덱스 연산자(indexed access operator)
게터/세터를 다루기 위한 대괄호([]) 연산자를 제공

| 표현식 | 함수 의미 |
| --- | --- |
| a[i] | a.get(i) |
| a[i, j] | a.get(i,j) |
| a[i_1, ..., i_n] | a.get(i+1, ..., i_n) |
| a[i] = b | a.set(i, b) |
| a[i, j] = b | a.set(i, j, b) |
| a[i_1, ..., i_n] = b | a.set(i_1, ..., i_n, b) |


### 단일 연산자
먼저 a의 자료형을 결정하고 매개변수 없이 각 연산자에 대한 함수를 호출한 다음 연산된 결과를 반환

| 표현식 | 함수 의미 |
| --- | --- |
| +a | a.unaryPlus() |
| -a | a.unaryMinus() |
| !a | a.not() |

```javascript
data class Point(val x: Int, val y: Int)

operator fun Point.unaryMinus() = Point(-x, -y)

val point = Point(10, 20)
println(-point) // 단일 연산자에 의해 (-10, -20) 값을 바꿈
```


### 포함 범위 연산자

in 연산자는 반복문에 사용해 특정 객체의 반복에 사용 혹은 포함여부 판단
```javascript
if (i in 1..10) { // 1 <= && i <= 10 와 동일
    println(i)
}
for (i in 1..4) print(i) // "1234" 출력
```

| 표현식 | 함수 의미 |
| --- | --- |
| a in b | b.contains(a) |
| a !in b | !b.contains(a) |


### 할당 연산자(augmented assignment)
예를 들어 a += b는 a + b의 연산 결과를 다시 a에 할당

| 표현식 | 함수 의미 |
| --- | --- |
| a += b | a.plusAssign(b) |
| a -= b | a.minusAssign(b) |
| a *= b | a.timesAssign(b) |
| a /= b | a.divAssign(b) |
| a %= b | a.remAssign(b), a.modAssign(b)(지원중단) |


### 동등성 연산자(equality and inequality)

| 표현식 | 함수 의미 |
| --- | --- |
| a == b | a?.eqauls(b) ?: (b === null) |
| a != b | !(a?.eqauls(b) ?: (b === null)) |

### 비교 연산자(comparison)

| 표현식 | 함수 의미 |
| --- | --- |
| a > b | a.compareTo(b) > 0 |
| a < b | a.compareTo(b) < 0 |
| a >= b | a.compareTo(b) >= 0 |
| a <= b | a.compareTo(b) <= 0 |
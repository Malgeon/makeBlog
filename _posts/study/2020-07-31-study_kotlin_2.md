---
layout: post
author: study
title:  "Kotlin 함수편 - [2]"
description: "자료형 변환, 스마트 캐스트, 비트 매서드"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 

## 자료형 변환

- 변환 메서드 사용
```javascript
var a: Int = 1
var b: Double = a.toDouble()
```

- 표현식에서 자동 변환
```javascript
var result = 1L + 3// Long + Int => result: Long
```

## 이중 등호와 삼중 등호

```javascript
var a: Int = 128
var b: Int = 128

println(a == b) // true
println(a === b) // false
```
```javascript
var a: Int = 128
var b: Int? = 128

println(a == b) // true
println(a === b) // false
```
```javascript
var a: Int = 128
var b: Int? = 128

println(a == b) // true
println(a === b) // false
```

```javascript
var a: Int = 128

var c: Int? = a
var d: Int? = a
var e: Int? = c

println(c == d) // true
println(c === d) // false
 
println(c == e) // true
println(c === e) // true
```

## 스마트 캐스드 Number
Number로 선언함으로써 Int, Long, Float 등 숫자 자료형을 자동으로 변환해준다.
```javascript
var a: Number = 12.2
a = 12
a = 120L
a += 12.0f
```

## 스마트 캐스트 Any
자료형이 정해지지 않는 경우 사용. 언제든 필요한 자료형으로 자동 변환
```javascript
var a: Any = 1
a = "Hello"
a = 120L
println("a: $a type: ${a.javaClass}") // a: 120 type: long
```

## is 키워드
```javascript
var num = 256

if (num is Int) {
  print(num) 
} else if(num !is Int) { // !(num is Int)와 동일
  print("Not a Int")
}
```


## 비트 연산을 위한 비트 매서드

| 표현식 | 설명 |
| :---: | :---: |
| 4.shl(bits) | 4를 표현하는 비트를 bits만큼 왼쪽으로 이동(부호 있음) |
| 7.shr(bits) | 7를 표현하는 비트를 bits만큼 오른쪽으로 이동(부호 있음) |
| 12.ushr(bits) | 12를 표현하는 비트를 bits만큼 오른쪽으로 이동(부호 없음) |
| 9.and(bits) | 9를 표현하는 비트를 bits를 표현하는 비트로 논리곱 연산 |
| 4.or(bits) | 4를 표현하는 비트를 bits를 표현하는 비트로 논리곱 연산 |
| 24.xor(bits) | 24를 표현하는 비트를 bits를 표현하는 비트로 배타적 연산 |
| 78.inv(  ) | 78를 표현하는 비트를 모두 뒤집음 |



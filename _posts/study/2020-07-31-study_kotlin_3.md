---
layout: post
author: study
title:  "Kotlin 함수편 - [3]"
description: "Kotlin 함수편 기본 - 함수"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 

## 함수의 선언

```javascript
fun sum(a: Int, b: Int): Int {
  var sum = a + b
  return sum
}
```

fun 함수이름([변수 이름: 자료형, 변수 이름: 자료형..]) : [반환값의 자료형] {
  ...
  [return 반환값]
}

반환값이 없는 함수일 경우 반환값의 자료형으로 unit을 사용한다. 이 함수는 반환값이 없다는 unit값을 반환한다. (자바에서 void와 비슷하지만 void는 변환값이 전혀 없다.) 그리고 Unit은 생략이 가능하다.

```javascript
fun outName(name: String): Unit {
  println("Name: $name")
  return Unit
}
```

```javascript
fun outName(name: String) {
  println("Name: $name")
}
```


- 최상위 함수(Top-level): 소스코드 상위에 있는 함수. 
어느 곳에서 선언하든 사용가능하다.
- 지역 함수 : 소스코드 내에 선언 된 함수.
해당 함수를 사용하기 전에는 무조건 선언되어야 한다.


## 간략화된 함수

함수는 아래와 같이 간략화 하여 사용할 수 있다.

```javascript
fun sum(a: Int, b: Int): Int = a + b 
```

이때 반환값이 추론이 가능할 경우 반환값의 자료형은 생략 가능하다. (간략화 하지 않을 경우는 불가능)

```javascript
fun sum(a: Int, b: Int) = a + b 
```

```javascript
fun outName(name: String) = println("Name: $name")
```

## Default 매개변수
매개변수에 Default 값을 설정할 수 있다.
매개변수 중 특정 매개변수 만 값을 설정할 경우 함수의 매개변수에 값을 직접 넣는다.

```javascript
fun sum(a: Int, b: Int = 5): Int = a + b 
println(sum(2)) // 7
```

```javascript
fun sum(a: Int = 2, b: Int = 5): Int = a + b 
println(sum(b=2)) // 4
```

## 가변변수
매개변수가 가변적일 경우 동일한 자료형에 대해 vararg를 이용하여 사용이 가능하다.
```javascript
fun normalVarargs(vararg a: Int) {
    for (num in a) {
        print("$num ")
    }
}
fun main() {
    normalVarargs(1) // 1
    println()
    normalVarargs(1, 2, 3, 4, 5) // 1 2 3 4
}

```
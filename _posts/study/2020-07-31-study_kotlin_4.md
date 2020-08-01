---
layout: post
author: study
title:  "Kotlin - [4]"
description: "Kotlin 함수형 프로그래밍"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 

## 코틀린은 다중 패러다임 언어
- 함수형 프로그래밍(FP: Functional Programming)
- 객체 지향 프로그래밍(OOP: Object-Oriented Programming)


## 함수형 프로그래밍
- 코드 간략, 테스트나 재사용성 증가
- 람다식, 고차 함수를 사용해 구성
- 순수 함수


## 순수 함수(pure function)

+ 부작용(side-effect)이 없는 함수
   * 동일한 입력 인자에 대해서는 항상 같은 결과를 출력 혹은 반환 한다.
   * 값이 예측 가능해 결정적(deterministic)이다.

 ```javascript
 fun sum(a: Int, b: Int): Int {
     return a + b //도일한 인자인 a, b를  입력 받아 항상 a + b를 출력(부작용이 없음)
 }
 ``` 

+ 순수함수의 조건
    * 같은 인자에 대하여 항상 같은 값을 반환
    * 함수 외부의 어떤 상태도 바꾸지 않는다.

+ 순수함수가 아닌 예
 ```javascript
 fun check() {
     val test = User.grade() // check() 함수에 없는 외부의 User 객체를 사용
     if (test != null) process(test) // 변수 test는 User.grade()의 실행 결과에 따라 달라짐
 }
 ``` 

```javascript
const val global = 10

fun main() {
    val num1 = 10
    val num2 = 3
    val result = noPureFunction(num1, num2)
    println(result)
}

fun noPureFunction(a: Int, b: Int): Int {
    return a + b + global // global 값이 바뀔 때 마다 return 값이 달라진다. no Pure
}
``` 

+ 순수 함수가 필요한 이유
    + 입력과 내용을 분리하고 모듈화 하므로 재사용성이 높아진다.
        + 여러가지 함수들과 조합해도 부작용이 없다.
    + 특정 상태에 영향을 주지 않으므로 병행 작업 시 안전하다.
    + 함수의 값을 추적하고 예측 할 수 있기 떄문에 테스트, 디버깅 등이 유리하다.

+ 함수형 프로그래밍에 적용
    + 함수를 매개변수, 인자에 혹은 반환값에 적용(고차 함수)
    + 함수를 변수나 데이터 구조에 저장
    + 유연성 증가
  

### 람다식
#### 람다식(Lambda Expression)이란?
- 익명 함수의 하나의 형태로 이름 없이 사용 및 실행이 가능
- 람다 대수(Lambda calculus)로 부터 유래

`{ x, y -> x + y } // 람다식의 예 (이름이 없는 함수 형태)`

#### 람다식의 이용
- 람다식은 고차 함수에서 인자로 넘기거나 결과값으로 반환 등을 할 수 있다.

### 일급 객체
#### 일급 객체(First Class Citizen)란?
- 일급 객체는 함수의 인자로 전단할 수 있다.
- 일급 객체는 함수의 반환값에 사용할 수 있다.
- 일급 객체는 변수에 담을 수 있다.

`코틀린에서는 함수는 1급 객체로 다룬다.(1급 함수라고도 한다.)`

### 고차 함수
#### 고차 함수(high-order function)란?
```javascript
fun main() {
    println(highFunc({ x, y -> x + y}, 10, 20))
}

fun highFunc(sum: (Int, Int) -> Int, a: Int, b: Int): Int = sum(a, b)// sum 매개변수는 함수
``` 
차례로
higntFunc => 고차함수
sum => 람다식 매개변수
(Int, Int) -> Int => 자료형이 람다식으로 선언 되어 { x, y -> x + y } 형태로 인자를 받는 것이 가능
a: Int, b: Int) : Int => 매개 변수와 반환 자료형
sum(a, b) => return 값. 람다식 표현문에 따라 a + b

아래와 같이 바꿀 수 있다.

```javascript
fun main() {
    val result = highFunc(1, 3, {x, y -> x + y})
}

fun highFunc(a: Int, b: Int, sum: (Int, Int) -> Int): Int = sum(a, b)// sum 매개변수는 함수
``` 


```javascript
fun main() {
    val result = highFunc(1, 3) {x, y -> 
        x + y
    }
}

fun highFunc(a: Int, b: Int, sum: (Int, Int) -> Int): Int = sum(a, b)// sum 매개변수는 함수 자바스크립트 함수형!?
``` 

람다의 parameter가 하나인 경우 컴파일러가 추론이 가능하여 해당 값을 it으로 사용이 가능하다.

```javascript
fun more(out: (String) -> String): Unit {
    println(out("Hello"))
}
fun main() {
    more{"$it Kotlin"} //Hello Kotlin
}
``` 


### 인라인 함수

람다식을 이용하게 되면 런타임시 해당 람다식은 함수에 해당하는 객체로 변환되어 메모리 할당이 이루어 진다. 즉, 람다식이 많아지면 많아질수록 객체 변환이 많이 이루어 지게 되어 런타임 오버헤드를 유발한다. 이런 문제점을 해결하는 방법으로 람다식에 inline을 붙임으로써 함수 호출 방식이 아닌 코드 자체를 복사하여 붙여넣는 방식으로 컴파일이 된다. 
그렇지만 함수를 만들다 보면 inline 방식을 이용하고 싶은 매개변수가 생길수도 있는데, 그럴 경우 해당 매개변수 앞에 noinline을 붙이면 inline이 아닌 방식으로 동작을 하게 된다.
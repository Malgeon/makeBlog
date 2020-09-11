---
layout: post
author: study
title:  "Kotlin 함수편 - [4]"
description: "함수형 프로그래밍, 순수함수, 람다식, 고차함수"
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

람다식으로 변수 선언 또한 가능하다. 이때. 선언된 변수는 함수처럼 사용이 가능하다.

```javascript
val result: Int
val multi = { a: Int, b: Int -> a * b}
/** 아래는 모두 동일한 표현 방법이다.
val multi: (Int, Int) -> Int = { a: Int, b: Int -> a * b}
val multi = { a: Int, b: Int -> a * b}
val multi: (Int, Int) -> Int = { a, b -> a * b} 
물론 모두 생략하는 것은 추론이 가능하지 않아서 안된다. 
*/
result = multi(10, 20) //함수 처럼 사용이 가능하다.
``` 

표현식이 2줄 이상일 경우는 마지막 표현식이 반환된다.

```javascript
val result: Int
val multi: (Int, Int) -> Int = { a, b -> 
    println("a*b")
    a * b // 만일 반환값이 Int로 설정 되어 있는데 a * b가 생략된다면 Error
}
result = multi(10, 20) //200
``` 

반환 자료형이 없거나 매개변수가 하나 있을 경우

```javascript
val greet: ()->Unit = { println("Hello World!") } // 추론이 가능하므로 자료형 생략
val square: (Int)->Int = { x -> x * x } 
// 선언부 생략하고 싶다면 x의 자료형 명시
val square = { x: Int -> x * x } 
``` 

람다식 안에 람다식

```javascript
val nestedLambda: ()->()->Unit = { { println("nested") } }  // 추론 가능하므로 생략 가능하다.
val nestedLambda = { { println("nested") } }
``` 

### 고차함수 이용
다음과 같은 예시를 살펴보자.

```javascript
fun sum(a: Int, b: Int) = a + b

fun mul(a: Int, b: Int): Int {
    return a * b
}

fun funcFunc(a: Int, b: Int) = sum(a, b) // 함수를 return

fun main() {
    val result = sum(10, 10)
    val result2 = mul( sum(10, 5), 10) // 매개변수에 함수 사용
    val result3 = funcFunc(2,3)
}
``` 

람다식을 이용

### 함수 호출

CallByValue

```javascript
fun main() {
    val result = callByValue(lambda())
    println(result)
}

fun callByValue(b: Bollean): Boolean {
    println("callByValue function")
    return b // b는 람다식함수의 결과값
}

val lambda: () -> Boolean = {
    println("lambda function") 
    true
}
```
순서: lambda 바로 실행 결과 값 반환, 반환된 결과값을 callByValue 매개변수에 복사, return.

CallByName

```javascript
fun main() {
    val result = callByName(otherLambda)
    println(result)
}

fun callByName(b: () -> Bollean): Boolean { //b는 람다식 함수
    println("callByName function")
    return b() // b는 람다식함수
}

val otherLambda: () -> Boolean = {
    println("lambda function") 
    true
}
```
순서: otherLambda가 callByName 매개변수에 복사, return에서 b()로 람다식 함수가 호출. 람다식 true return, callByname return.

callByReference

```javascript
fun sum(x: Int, y: Int) = x + y

//funcParam(3, 2, sum) => error

fun funcParam(a: Int, b: int, c: (Int, int) -> Int): Int {
    return c(a, b)
}
funcParam(3, 2, ::sum) //일반함수의 내용물이 참조하고자 하는 람다식과 완전히 일치하는 경우 사용 가능.
```
### 매개변수 개수에 따라 람다식 구성 방법
매개변수 없는 경우
```javascript
fun main() {
    // 매개변수 없는 람다식 함수
    noParam( { "Hello World!" } )
    noParam { "Hello World!" } //위와 동일한 결과.
}

fun noParam( out: () -> String) = println(out())
```

매개변수 한 개

```javascript
fun main() {
    oneParam( {  a ->  "Hello World! $a" } )
    oneParam { a -> "Hello World! $a" } 
    oneParam { "Hello World! $it" } // 매개변수가 하나 일 경우 $it을 사용함으로써 화살표 식을 줄일수 있다. 여러 개일 경우 사용 불가능
    //전체 동일한 결과
}

fun oneParam( out: (String) -> String) = println(out("OneParam"))
```

매개변수 두 개 이상

```javascript
fun main() {
    moreParam{ a, b ->  "Hello World! $a $b" }
}

fun moreParam( out: (String, String) -> String) = println(out("OneParam", "TwoParam"))
```

만일 앞선 매개변수를 사용하지 않을 예정이라 생략하고 싶다면, 언더바를 사용하면 된다.

```javascript
fun main() {
    moreParam{ _, b ->  "Hello World! $b" }
}
```

일반 매개변수와 람다식 매개변수를 같이 사용

```javascript
fun main() {
    withArgs("Arg1", "Arg2", { a, b -> "Hello World! $a $b"} )
    withArgs("Arg1", "Arg2") { a, b -> "Hello World! $a $b"} 
    //동일한 표현, 인자와 람다식을 분리해서 보게 할 수 있다.
}

fun withArgs(a: String, b: String, out: (String, String) -> String) {
    println(out(a, b))
}
/**람다식이 매개변수 중 앞에 위치할 경우 불가능.

withArgs { a, b -> "Hello World! $a $b"} ("Arg1", "Arg2") 
fun withArgs(out: (String, String) -> String, a: String, b: String) {
    println(out(a, b))
} 
안됨!
*/
```

두 개 이상의 람다식을 가질 때

```javascript
fun main() {
    twoLambda({ a, b ->  "First $a $b" }, {"Second $it"})
    twoLambda({ a, b ->  "First $a $b" }) {"Second $it"} // 두 개 이상일 경우 마지막 람다식에 한에서 중괄호로 빼낼수 있다.
}

fun twoLambda(first: (String, String) -> String, second: (String) -> String) {
    println(first("OneParam", "TwoParam"))
    println(second("OneParam"))
}
```

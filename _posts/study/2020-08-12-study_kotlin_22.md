---
layout: post
author: study
title:  "Kotlin 객체편 - [12]"
description: "제네릭"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 제네릭(generic)
자료형의 객체들을 다루는 메서드나 클래스에서 컴파일 시간에 자료형을 검사해 적당한 자료형을 선택할 수 있도록 하기 위해 만들어진 개념


제네릭의 일반적인 사용
- 앵글 브래킷(<>) 사이에 형식 매개변수를 사용해 선언한다
- 형식 매개변수는 자료형을 대표하는 용어로 T와 같이(Type의 약자) 특정 영문의 대문자로 사용한다


```javascript
class Box<T>(a: T) {
    var name = a
}

fun main() {
    val box1 = Box(1)
    val box1: Box<Int> = Box(1) // 추론이 가능하기 때문에 생략 가능하다
    val box2 = Box("kildong")
}
```

```javascript
class Box<T>(t: T) { // 제네릭을 사용해 형식 매개변수를 받아 name에 저장
    var name = t
}
fun main() {
    val box1: Box<Int> = Box<Int>(1)
    val box2: Box<String> = Box<String>("Hello")

    println(box1.name) // 1
    println(box2.name) // Hello
}
```

### 형식 매개변수의 이름
강제사항은 없으나 일종의 규칙처럼 사용된다

| 형식 매개변수 이름 | 의미 |
| --- | --- |
| E | 요쇼(element) |
| K | 키(key) |
| N | 숫자(number) |
| T | 형식(type) |
| V | 값(value) |
| S, U, V etc. | 두 번째, 세 번째, 네 번째 형식(2nd, 3rd, 4th types) |


자료형이 추론 가능한 경우는 앵글 브라켓 표현을 생략해도 된다.
```javascript
val box3 = Box(1) // 1은 Int형이므로 Box<Int>로 유추한다.
val box4 = Box("Hello") // "Hello"은 String형이므로 Box<String>로 유추한다.
```


형식 매개변수를 한개 이상 받는 클래스
- 인스턴스를 생성하는 시점에서 클래스의 자료형을 정하게 된다.
- 제네릭 클래스 내에 메서드에도 다음과 같이 형식 매개변수를 사용

```javascript
class MyClass<T> { // 한 개의 형식 매개변수를 가지는 클래스
    fun myMethod(a: T) { // 메서드의 매개변수 자료형에 사용됨
        ...
    }
}
```

- 프로퍼티에 지정하는 경우
    - 주 생성자나 부 생성자에 형식 매개변수를 지정해 사용


주 생성자의 형식 매개변수
```javascript
class MyClass<T>(val myProp: T) { } // 주 생성자의 프로퍼티
```

부 생성자의 형식 매개변수
```javascript
class MyClass<T> {
    val myProp: T // 프로퍼티
    constructor(myProp: T) { // 부 생성자 이용
        this.myProp = myProp
    }
}
```

정수형이 사용된 객체 초기화 예시
```javascript
var = MyClass<Int>(12) // 주 생성자 myProp에는 12가 할당되며 정수형으로 결정됨
println(a.myProp) // 12
println(a.javaClass) // MyClass
```


```javascript
open class Parent

class Child : Parent()
class Cup<T>

fun main() {
    val obj1: Parent = Parent()
    // val obj2: Child = Parent() // 에러!
    val obj3: Child = Child()
    val obj4: Parent = Child() // 된다!

    val obj5 = Cup<Parent>()
    // val obj5: Cup<Parent> = Cup<Parent>() 동일하다.
    // val obj6: Cup<Child> = Cup<Parent>() // 에러!
    val obj7 = Cup<Child>()
    // val obj8: Cup<Parent> = Cup<Child>() // 에러! -

    val obj8: Cup<Child> = obj7 // 형식이 일치하므로 가능!
}
```
여기서 in 과 out을 이용하면 가변성을 줄수 있다. (나중에 정리)


### 형식 매개변수의 null 제어
제네릭의 형식 매개변수는 기본적으로 null 가능한 형태로 선언된다.
```javascript
class GenericNull<T> { // 기본적으로 null이 허용되는 형식 매개변수
    fun EqualityFunc(arg1: T, arg2: T) {
        println(arg1?.equals(arg2))
    }
}

fun main(args: Array<String>) {
    val obj = GenericNull<String>() // non-null로 선언됨
    obj.EqualityFUnc("Hello", "World") // null이 허용되지 않음

    val obj2 = GenericNull<Int?>() // null이 가능한 형식으로 선언됨
    obj2.EqualityFunc(null, 10) // null 사용
}
```

null을 허용하지 않으려면 특정 자료형으로 제한하면 된다.
 -> `<T: Any>`

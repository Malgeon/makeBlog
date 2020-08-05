---
layout: post
author: study
title:  "Kotlin 객체편 - [1]"
description: "Kotlin 객체 지향 프로그래밍 기본 - 1"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### 생성자

```
class 클래스명 constructor(필요한 매개변수들..) { // 주 생성자의 위치
    ...
    constructor(필요한 매개변수들..) { // 부 생성자의 위치
        // 프로퍼티의 초기화
    }

    [constructor(필요한 매개변수들..) { ... }] // 추가 부 생성자
    ...
}
```

- 주 성생자(Primary constructor)
클래스명과 함께 기술되며 보통의 경우 constructor 키우드를 생략할 수 있다.

```javascript
class Bird (var name: String, val wing: Int, var beak: String) {
    /*var name: String = _name
    val wing: Int = _wing
    var beak: String = _beak*/
    /*constructor(_name: String, _wing: Int, _beak: String) {
        name = _name
        wing = _wing
        beak = _beak
    }*/
    init {
        println("-----------init start ------------")
        name = name.capitalize()
        println("name: $name, wing: $wing, break: $break")
        println("-----------init end   ------------")
    }
    fun fly() {
        println("Fly")
    }
}
fun main() {
    val coco = Bird("coco", 2, "long")
    coco.fly()
}
```
### init{} 초기화 블록
객체가 생성될 때 자동으로 실행되는 block을 설정할수 있다. 해당 블록의 작업시간이 길어질수록 객체 생성 시간이 늘어나므로 간단한 코드를 사용한다.

- 부 생성지(Secondary Constructor)
클래스 본문에 기술되며 하나 잇아의 부 생성자를 정의할 수 있다.


```javascript
class Bird {
    var name: String
    val wing: Int
    var beak: String

    constructor(_name: String, _wing: Int, _beak: String) {
        name = _name
        wing = _wing
        beak = _beak
    }

    constructor(_name: String, _beak: String) {
        name = _name
        wing = 2
        beak = _beak
    }
    fun fly() {
        println("Fly")
    }
}
fun main() {
    val coco = Bird("coco", 2, "long")
    val coco2 = Bird("coco", "short")
    coco.fly()

    println("coco2: name: ${coco2.name}, wing: ${coco2.wing}, beak: ${coco2.beak}")
}
```


### 상속

```
open class 기반 클래스명 { // open으로 파생 가능 (다른 클래스가 상속 가능한 상태가 됨)
    ...
}
class 파생 클래스명 : 기반 클래스명() { // 기반 클래스로 부터 상속, 최종 클래스로 파생 불가 ( : 사이로 빈칸이 양쪽으로 존재해야 한다!!)
    ...
}
```

코틀린의 모든 클래스는 묵시적으로 Any로부터 상속


```javascript
open class Bird(var name: String, var wing: Int, var beak: String) {
    fun fly () {
        println("Fly")
    }
}

class Lark(name: String, wing: Int, beak: String) : Bird(name, wing, beak){
    fun singHitone() {
        println("sing Hitone")
    }
}

class Parrot : Bird {
    var language: String
    constructor(name: String, wing: Int, beak: String, language: String) : super(name, wing, beak) {
        this.language = language
    }
    fun speak() {
        println("Speak: $language")
    }
}

fun main() {
    val lark = Lark("myLark", 2, "short")
    val parrot = Parrot("myParrot", 2, "long", "English")
}
```


### 다형성(polymorphism)
같은 이름을 사용하지만 구현 내용이 다르거나 매개변수가 달라서 하나의 이름으로 다양한 기능을 수행할 수 있는 개념

Static Polymorphism
- 컴파일 타임에 결정되는 다형성
- 단순하게 보면 메서드 overloding을 사용할 때
```
fun example(a: Int)
fun example(a: Int, b: Int)
```

Dynamic Polymorphism
- 런타임 다형성
- 동적으로 구성되는 overrideing된 메서드를 사용할 때
```
open class Parent (name: Int) {
    open fun example()
}
class Child1(name: Int) : Parent(name) {
    override fun example()
}
class Child2(name: Int) : Parent(name) {
    override fun example()
}

var child1 = Child1(1)
var child2 = Child1(1)

child1.example()
child2.example()
```


overriding
- 기능을 완전히 다르게 바꿔 재설계
- 누르다 -> 행위 -> push()
- push()는 '확인' 혹은 '취소' 용도로 서로 다른 기능을 수행 할 수 있음

overloading
- 기능은 같지만 인자를 다르게 하여 여러 경우를 처리
- 출력한다 -> 행위 -> print()
- print(123), print("Hello") 인자는 다르지만 출력의 기능은 동일함



```javascript
open class Bird(var name: String, var wing: Int, var beak: String) {
    open fun fly () {
        println("Fly")
    }
}

class Lark(name: String, wing: Int, beak: String) : Bird(name, wing, beak){
    fun singHitone() {
        println("sing Hitone")
    }

    override fun fly () {
        println("Quick Fly")
    }
}

class Parrot : Bird {
    var language: String
    constructor(name: String, wing: Int, beak: String, language: String) : super(name, wing, beak) {
        this.language = language
    }
    fun speak() {
        println("Speak: $language")
    }

    override fun fly () {
        println("Slow Fly")
    }
}

fun main() {
    var lark = Lark("myLark", 2, "short")
    var parrot = Parrot("myParrot", 2, "long", "English")

    lark.fly() // Quick Fly
    lark.singHitone()

    parrot.fly() // Slow Fly
    parrot.speak()

    var lark: Bird = Lark("myLark", 2, "short")
    var parrot: Bird = Parrot("myParrot", 2, "long", "English")

    lark.fly() // 동일하게 Quick Fly
    // lark.singHitone() error

    parrot.fly() // 동일하게 Slow Fly
    // parrot.speak() error
}
```

```javascript
open class Lark() : Bird() { // open! 상위클래스 이다. 
    final override fun sing() { // 하위 클래스에서 override 금지
        /* 구현부를 새롭게 재정의 */
    }
}
```
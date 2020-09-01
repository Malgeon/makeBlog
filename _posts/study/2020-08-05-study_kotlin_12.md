---
layout: post
author: study
title:  "Kotlin 객체편 - [2]"
description: "this, super, @, <>"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### this & super

| super | this | 
| --- | --- |
| super.프로퍼티명 // 상위 클래스의 프로퍼티 참조 | this.프로퍼티명 // 현재 클래스의 프로퍼티 참조 |
| super.매서드명() // 상위 클래스의 메서드 참조 | this.메서드명() // 현재 클래스의 메서드 참조 |
| super() // 상위 클래스의 생성자 참조 | this() // 현재 클래스의 생성자 참조 |

```javascript
open class Bird(var name: String, var wing: Int, var beak: String, var color: String) {
    fun fly() = println("Fly wing: $wing")
    open fun sing(vol: Int) = println("Sing vol: $vol")
}

class Parroy(name: String, wing: Int = 2, beak: String, color: String, 
            var language: String = "natural") : Bird(name, wing, beak, color) {
    fun speak() = println("Speak! $language")

    override fun sing(vol: Int) { // 부모의 내용과 새로 구현된 내용을 가진다.
        super.sing(vol)  // 상위 클래스의 sing()을 먼저 수행한다.
        println("I'm a parrot! the volume level is $vol")
        speak()
    }
}
```

this와 super를 사용하는 부 생성자 예시
```javascript
open class Person {
    constructor(firstName: String) {
        println("[Person] firstName: $firstName")
    }
    constructor(firstName: String, age: Int) {
        println("[Person] firstName: $firstName, $age") // 첫번째 실행
    }
}

class Developer : Person {
    constructor(firstName: String): this(firstName, 10) { // 아래 constructor를 참조하게 된다. 마지막에 실행
        println("[Developer] $firstName")
    }
    constructor(firstName: String, age: Int): super(firstName, age) {
        println("[Developer] $firstName, $age") // 상위 클래스 constructor 참조. 두번째 실행
    }
}

fun main() {
    val sean = Developer("Sean")
}
```

```javascript
class Person(firstName: String, out: Unit = println("[Primary constructor] Parameter")) { // 3
    val fName = println("[Property] Person fName: $firstName") // 4
    init { // 5
        println("[init] Person init block")
    }

    constructor(firstName: String, age: Int, // 1
                out:Unit = println("[Secondary Constructor] Parameter")) this(firstName) { // 2
        println("[Secondary Constructor] Body: $firstName, $age") // 6
    }
}

fun main() {
    val p1 = Person("Kildong", 30)
    println()
    // val p2 = Person("Dooly")
}
```

### @ 기호
이너 클래스에서 바깥 클래스의 상위 클래스를 호출하려면 super 키워드와 함께 @ 기호 옆에 바깥 클래스명을 작성해 호출한다.

```javascript
open class Base {
    open val x: Int = 1
    open fun f() = println("Base Class f()")
}

class Child : Base() {
    override val x: Int = super.x + 1
    override fun f() = println("Child Class f()")

    inner class Inside {
        fun f() = println("Inside Class f()")
        fun test() {
            f() // 1. 현재 이너 클래스 f() 접근
            Child().f() // 2. 바로 바깥 클래스인 Child 클래스 f() 접근
            super@Child.f() // 3. Child의 상위 클래스인 Base 클래스 f() 접근
            println("[Inside] super@Child.x: ${super@Child.x}") // 4. Base의 x 접근
        }
    }
}

fun main() {
    val c1 = Child()
    c1.Inside().test() // 이너 클래스 Inside의 메서드 test() 실행
}
```

### <> 을 사용한 이름 중복 해결

```javascript
open class A {
    open fun f() = println("A Class f()")
    fun a() = println("A Class a()")
}
interface B {
    fun f() = println("B Interface f()")
    fun b() = println("B Interface b()")
}
class C : A(), B {
    override fun f() = println("C Class f()")
    fun test(() {
        f() // 현재 클래스의 f()
        b() // 인터페이스 B의 b()
        super<A>.f() // A 클래스의 f()
        super<B>.f() // 인터페이스 B의 f()
    }
}
fun main() {
    val c = C()
    c.test()
}
```
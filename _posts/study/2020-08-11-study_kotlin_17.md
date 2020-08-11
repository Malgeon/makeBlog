---
layout: post
author: study
title:  "Kotlin 객체편 - [7]"
description: "Kotlin 클래스 - 추상클래스와 인터페이스"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 추상클래스와 인터페이스 

### 추상클래스(abstract class)
- 구현 클래스에서 가져야할 명세를 정의한 클래스(프로퍼티 및 메서드 템플릿)
- abstract라는 키워드와 함꼐 선언하며 추상클래스는 객체 생성 안됨
- '구체적이지 않는 것'을 나타내기 때문에 하위 파생 클래스에서 구체적으로 구현

```
abstarct class Vehicle
```

- open 키워드를 사용하지 않고도 파생 클래스 작성 가능

```javascript
// 주 생성자에 정의된 프로퍼티는 비 추상(non-abstract) 프로퍼티들이다.
abstract class Vehicle(val name: String, val color: String, val weight: Double) {
    // 추상 프로퍼티 - 하위 클래스에서 반드시 오버라이딩
    abstract val maxSpeed: Double
    // 비 추상 프로퍼티
    var year: String = "2008" // 인터페이스와는 다르게 상태를 저장할 수 있다.
    
    // 추상 메서드
    abstract fun start()
    abstract fun stop()

    //비 추상 메서드
    fun displaySpec() {
        println("name: $name, color: $color, weight: $weight, year: $tear, maxSpeed: $maxSpeed") // 추상 프로퍼티도 사용 가능
    }
}

class Car(name: String, 
          color: String, 
          weight: Double, 
          override val maxSpeed: Double) : Vehicle(name, color, weight) {
     override fun start() {
         println("Car Started")
     }

     override fun stop() {
         println("Car Stopped")
     }

     fun autoPilotOn() {
         println("Auto Pilot ON")
     }
}

fun main() {
    var car = Car("Matiz", "red", 1000.0, 100.0)
    car.year = "2014" // 추상 클래스의 비 추상 프로퍼티 사용
    car.displaySpec()
}

```

### 단일 인스턴스로 객체를 생성

object를 사용한 생성
- 추상 클래스로부터 하위 클래스를 생성하지 않고 단일 인스턴스로 객체 생성 가능
```javascript
abstract class Printer {
    abstract fun print() // 추상 메서드
}

val myPrinter = object: Printer() { // 객체 인스턴스
    override fun print() { // 추상 메서드의 구현
        println("출력합니다.")
    }
} // 하위 클래스를 선언해서 상속하고 다시 인스턴스를 만드는 과정을 생략 할 수 있다. 임시적으로 사용할때 유용하다.

fun main() {
    myPrinter.print()
}
```


### 인터페이스(interface)

계약서의 개념과 비슷하다.
- 계약서에는 무엇 무엇을 하라는 추상적인 활동들이 적혀 있다.
- 이것을 어떤 '작업자'가 받아들였다면 계약서에 있는 활동에 구체적인 내용을 반드시 실행 해야 한다.
- 그러나 계약서 자체로는 실행될 수 없다.

추상클래스와는 다른점
- 클래스가 아니므로 다양한 인터페이스로부터 클래스 구현 가능(다중 상속 지원, 클래스는 상속이 하나만 된다.)
- 추상 클래스와는 다르게 강한 연관을 가지지 않는다.


코틀린에서 인터페이스
- 다른 언어와는 다르게 기본적인 구현 내용이 포함될 수 있다.
    - 자바 버전 8 에서는 default 키워드를 사용해야만 구현 내용을 포함 할 수 있다.
- 선언하려면 interface 키워드를 사용해 정의
- 상속한 하위 클래스에서는 override를 사용해 해당 메서드를 구현

```
interface 인터페이스명 [: 인터페이스명...] {
    추상 프로퍼티 선언
    추상 메서드 선언

    [일반 메서드 선언 { ... }] // 이게 가능하다!
}
```

```javascript
interface Pet {
    var category: String // abstract 키워드가 없어도 기본은 추상 프로퍼티
    fun feeding() // 마찬가지로 추상 메서드
    fun patting() { // 구현부를 포함하면 일반 메서드로 지정된다.
        println("Keep patting!") // 구현부
    }
}

class Cat(override var category: String) : Pet {
    override fun feeding() {
        println("Feed the cat a tuna can!")
    }   
}

fun main() {
    val obj = Cat("small")
    println("Pet Category: ${obj.category}")
    obj.feeding() // Cat에서 구현한 메서드
    obj.patting() // Pet에서 구현한 일반 메서드
}
```


인터페이스에서는 프로퍼티에 값을 저장할 수 없다.(추상클래스는 가능) 그러나 val로 선언된 프로퍼티는 게터를 통해 필요한 내용을 구현할 수 있다.
```javascript
interface Pet {
    var category: String
    val msgTags: String // val 선언 시 게터의 구현이 가능
        get() = "I'm your lovely pet!"

    fun feeding()
    fun patting() {
        println("Keep patting!")
    }
}
...
println("Pet Message Tags: ${obj.msgTags}")
...
```

인터페이스의 상속 예시
```javascript
open class Animal(val name: String)

// feeding의 구현을 위해 인터페이스 Pet 지정
class Dog(name: String, override var category: String) : Animal(name), Pet {
    override fun feeding() {
        println("Feed the dog a bone")
    }
}
class Master {
    fun playWithPet(dog: Dog) { // 각 애완동물 종류에 따라 오버로딩 된다.
        println("Enjoy with my dog.")
    }
    fun playWithPet(cat: Cat) { // 고양이를 위한 메서드
        println("Enjoy with my cat")
    }
}

fun main() {
    val master = Master()
    val dog = Dog("Toto", "Small")
    val cat = Cat("Coco", "BigFat")
    master.playWithPet(dog)
    master.playWithPet(cat)
}
```

이 때, 만일 여러 애완동물이 있게되면 playWithPet()은 계속해서 오버로딩 해야 한다. 그렇게 되면 계속해서 동일한 코딩을 중복해서 구현해 줘야 하는 번거로움이 생기게 된다.
이런 번거로움을 해결해 보자.

```javascript
interface Pet {
    var category: String
    var species: String // 추가 구현
    val msgTags: String 
        get() = "I'm your lovely pet!"

    fun feeding()
    fun patting() {
        println("Keep patting!")
    }
}

open class Animal(val name: String)

class Dog(name: String, override var category: String) : Animal(name), Pet {
    override var species: String = "dog"
    override fun feeding() {
        println("Feed the dog a bone")
    }
}
class Master {
    fun playWithPet(pet: Pet) { // 인터페이스 객체로 매개변수를 지정한다.
        println("Enjoy with my ${pet.species}.")
    }
}

fun main() {
    val master = Master()
    val dog = Dog("Toto", "Small")
    val cat = Cat("Coco", "BigFat")
    master.playWithPet(dog)
    master.playWithPet(cat)
}
```

클래스는 기본적으로 다중 상속을 지원하지 않지만, 인터페이스는 인터페이스 여러개를 하나의 클래스에서 구현하는 것이 가능하므로 다중 상속과 같은 효과를 가진다.

다중 상속 예시
```javascript
interface Bird {
    val wings: Int
    fun fly()
    fun jump() = println("bird jump!")
}

interface Horse {
    val maxSpeed: Int
    fun run()
    fun jump() = println("jump!, max speed: $maxSpeed")
}

class Pegasus: Bird, Horse {
    override val wings: Int = 2
    override val maxSpeed: Int = 100
    override fun fly() = println("Fly!")
    override fun run() = println("Run!")
    override fun jump() = { // 추가적으로 jump 재정의
        super<Horse>.jump()
        println("and Jump!")
    }
}
```

인터페이스의 위임
```javascript
interface A {
    fun functionA(){}
}
interface B {
    fun functionB(){}
}
class C(val a: A, val b: B) {
    fun functionC() {
        a.functionA()
        b.functionB()
    }
}
```
```javascript
class DelegatedC(a: A, b: B): A by a, B by b {
    fun functionC() {
        functionA() // A의 위임
        functionA() // B의 위임
    }
}
```

위임을 사용하면 메서드를 쉽게 사용할 수 있다.

위임을 이용한 멤버 접근
```javascript
interface Nameable {
    var name: String
}

class StaffName : Nameable {
    override var name: String = "Sean"
}

class Work: Runnable { // 스레드 실행을 위한 인터페이스
    override fun run() {
        println("work...")
    }
}
// 각 매개변수에 해당 인터페이스를 위임한다.
class Person(name: Nameable, work: Runnable): Nameable by name, Runnable by work

fun main() {
    val person = Person(StaffName(), Work()) // 생성자를 사용해 객체 바로 전달
    println(person.name) // 여기서 StaffName 클래스의 name 접근
    person.run() // 여기서 Work 클래스의 run 접근
}
```
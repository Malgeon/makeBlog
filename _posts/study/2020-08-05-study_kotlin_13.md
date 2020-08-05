---
layout: post
author: study
title:  "Kotlin 객체편 - [3]"
description: "Kotlin 객체 지향 프로그래밍 기본 - 3"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 

### 캡슐화(encapsulation)

가시성 지시자(visibility modifiers)를 통해 외부 접근 범위를 결정
- private : 외부 접근 불가
- public : 어디서든 접근 가능(기본값)
- protected : 외부 접근 불가 그러나 하위 상속 요소에서는 가능
- internal : 같은 정의의 모듈 내부에서는 접근이 가능 (자바에서는 package 모듈은 패키지보다 상위 개념이다.)

선언 위치
```
[가시성 지시자] <val | var> 전역 변수명

[가시성 지시자] fun 함수명() { ... }

[가시성 지시자] [특정키워드] class 클래스명 [가시성 지시자] constructor(매개변수들) { 
    // 주 생성자에 가시성 지시자가 사용되면 constructor키워드를 생략할 수 없다.
    [가시성 지시자] constructor() { ... }
    [가시성 지시자] 프로퍼티들
    [가시성 지시자] 메서드들
}
```


### 상속 Test
```javascript
private class PrivateTest {
    private var i = 1
    private fun privateFunc() {
        i += 1
        println(i)
    }

    fun access() {
        privateFunc()
    }
}
class OtherClass {
    // val pc = PrivateTest() // class 자체가 private하므로 다른 클래스에서 public하게 할당 불가
    private val pc = PrivateTest() // 이렇게는 가능!

    fun test() { // 함수 내로 들어오게 되면 한번은 가려지게 되므로 할당이 가능하다.
        val testPc = PrivateTest()
        testPc.access()
    }
}

fun main() {
    val pc = PrivateTest()
    // pc.i = 3  // private이므로 접근 불가
    //pc.privateFunc() // 위와 동일하게 접근 불가
    pc.access()
}

fun topFunction() {
    fun localFunction() {
        val pc = PrivateTest() // 동일한 페이지 함수 내에서는 private 클래스 할당 가능.
        pc.access()
    }
}
```

protected
```javascript
open class Base {
    protected var i =1
    protected fun protectedFunc() {
        i += 1
        println(i)
    }
    fun access() {
        protectedFunc()
    }
    protected class Nested // 내부 클래스에는 지시자 허용
}

class Derived : Base() {
    var j = 1 + i
    fun derivedFunc(): Int {
        protectedFunc()
        return i
    }
    fun test(base: Base): Int {
        protectedFunc() 
        return i 
    }
}

class Other {
    fun other() {
        val base = Base()
        // base.i = 3 // 접근 불가
    }
}

fun main() {
    val base = Base()
    // base.i // 접근 불가
    // base.protectedFunc() // 접근 불가
    base.access()

    val derived = Derived()
    derived.j = 3
    derived.derivedFunc()
}
```

internal : 모듈 단위 - 프로젝트에 모듈을 만들지 않았다면 프로젝트 자체가 모듈이 된다.
```javascript
internal class InternalClass {
    internal var i = 1
    internal fun icFunc() {
        i += 1 // 접근 허용
    }
    fun access() {
        icFunc() // 접근 허용
    }
}

class Other {
    internal val ic = InternalClass() // 프로퍼티 지정시 internal로 맞춰야 한다.
    // val ic = InternalClass() // 불가능
    fun test() {
        ic.i // 접근 허용
        ic.icFunc() // 접근 허용
    }
}

fun main() {
    val mic = InternalClass() // 생성 가능
    mic.i // 접근 허용
    mic.icFunc() // 접근 허용
}
```

```javascript
fun main() { // 파일이 달라져도 접근 가능 (같은 모듈 내에서)
    val otheric = InternalClass()
    println(otheric.i)
    otheric.icFunc()
}
```

다이어그램에서 가시성 지시자

| 기호 | 가시성 지시자|
| :---: | :---: |
| - | private |
| # | protected |
| ~ | internal |
| + | public |


![가시성지시자와클래스의관계]({{ site.baseurl }}/assets/images/study/kotlin_oop_class.jpg)

```javascript
open class Base {
    // 이 클래스에서는 a, b, c, d, e 접근 가능
    private val a = 1
    protected open val b = 2
    internal val c = 3
    val d = 4 // 가시성 지시자 기본값은 public

    protected class Nested {
        // 이 클래스 에서는 a, b, c, d, e, f 접근 가능
        publiv val e: Int =  5
        private val f: Int = 6
    }
}
class Derived : Base() {
    // 이 클래스에서는 b, c, d, e 접근 가능
    // a 는 접근 불가
    oberride val b = 5 // Base의 'b' 는 오버라이딩됨 - 상위와 같은 protected 지시자
}

class Other(base: Base) {
    // base.a, base.b는 접근 불가
    // base.c와 base.d는 접근 가능(같은 모듈 안에 있으므로)
    // base.Nested는 접근 불가, Nested::e 역시 접근 불가
}
```


![관계]({{ site.baseurl }}/assets/images/study/kotlin_oop_relationship.JPG)

![다중성]({{ site.baseurl }}/assets/images/study/kotlin_oop_relationship_multiplicity.JPG)

![화살표표기법]({{ site.baseurl }}/assets/images/study/kotlin_oop_relationship_pointer.JPG)

![상태판단]({{ site.baseurl }}/assets/images/study/kotlin_oop_relationship_judge.JPG)


```javascript
// Association
class Patient(val name: String) {

    fun doctorList(d: Doctor) {
        println("Patient: $name, Doctor: ${d.name}")
    }
}

class Doctor(val name: String) {

    fun patientList(p: Patient) {
        println("Doctor: $name, Patient: ${p.name}")
    }
}

fun main() {
    val doc1 = Doctor("Kim")
    val patient1 = Patient("Kildong")
    doc1.patientList(patient1)
    patient1.doctorList(doc1)
}
```

```javascript
// Dependency
class Patient(val name: String, var id: Int) {

    fun doctorList(d: Doctor) {
        println("Patient: $name, Doctor: ${d.name}")
    }
}

class Doctor(val name: String, val p: Patient) {

    val customerId: Int = p.id

    fun patientList() {
        println("Doctor: $name, Patient: ${p.name}")
        println("Patient Id: $customerId")
    }
}

fun main() {
    val patient1 = Patient("Kildong", 1234)
    val doc1 = Doctor("Kim", patient1)
    doc1.patientList()
}
```

```javascript
// Aggregation
// 여러 마리의 오리를 위한 List 매개변수
class Pont(_name: String, _members: MutableList<Duck>) {
    val name: String = _name
    val members: MutableList<Duck> = _members
    constructor(_name: String): this(_name, mutableListOf<Duck>())
}

class Duck(val name: String)

fun main() {
    // 두 개체는 서로 생명주기에 영향을 주지 않는다.
    val pond = Pond("myFavorite")
    val duck1 = Duck("Duck1")
    val duck2 = Duck("Duck2")

    // 연못에 오리를 추가 - 연못에 오리가 집합한다
    pond.members.add(duck1)
    pond.members.add(duck2)

    // 연못에 있는 오리들
    for (duck in pond.members) {
        println(duck.name)
    }
}
```

```javascript
// Composition
class Car(val name: String, val power: String) {

    private var engine = Engine(power) // Engine 클래스 객체는 Car에 의존적

    fun startEngine() = engine.start()
    fun stopEngine() = engine.stop()
}

clas Engine(power: String) {
    fun start() = println("Engine has been started.")
    fun stop() = println("Engine has been stopped.")
}
fun main() {
    val car = Car("tico", "100hp")
    car.startEngine()
    car.stopEngine()
}
```

![객체간메세지전달]({{ site.baseurl }}/assets/images/study/kotlin_oop_message.JPG)
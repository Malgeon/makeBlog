---
layout: post
author: study
title:  "Kotlin - by 1. Property Delegation"
description: "코틀린의 Property Delegation 동작 분석"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### Delegation

코틀린에서는 두 가지의 Delegation을 소개한다.

[Property Delegation](https://kotlinlang.org/docs/reference/delegated-properties.html)과 [Implementation by Delegation](https://kotlinlang.org/docs/reference/delegation.html). 

이 둘은 각기 다른 기능을 가지고 있는데, 2개의 포스트로 각각의 기능을 이해해보려 한다.


### Property Delegation

프로퍼티 위임(Property Delegation)은 프로퍼티에 대한 getter/setter를 위임하여 위임받은 객체로 하여금 값을 읽고 쓸 때 어떠한 중간 동작을 수행하는 기능이다.

```
val/var <property name>: <Type> by <delegate>
```

이제 이름과 나이 그리고 연봉을 저장하는 프로그램에서 위임을 적용하는 과정을 보여주려고 한다.

```kotlin
class Person(val name: String, val _age: Int, val _salary: Int) {
    var age = _age
        get() {
            println("age get: $field")
            return field
        }
        set(value) {
            println("age set: $value")
            field = value
        }
    var salary = _salary
        get() {
            println("salary get: $field")
            return field
        }
        set(value) {
            println("salary set: $value")
            field = value
        }
}

fun main() {
    val p = Person("K", 20, 2000)
    p.age = 21
    p.salary = 2100
    println("${p.name} - age: ${p.age}, salary: ${p.salary}")
}
```

```
age set: 21
salary set: 2100
age get: 21
salary get: 2100
K - age: 21, salary: 2100 
```

간단한 프로그램임에도 코드가 많다. 여기서 코드를 줄이기 위해 중복되는 동작인 get과 set을 분리하여 호출하도록 만들었다.

```kotlin
class Delegator(val fname: String) {
    var value: Int =0
    fun getMethod(): Int {
        println("$fname get: $value")
        return value
    }
    fun setMethod(newValue: Int) {
        println("$fname set: $newValue")
        value = newValue
    }
}

class Person(val name: String) {
    val ageDelegator = Delegator("age")
    val salaryDelegator = Delegator("salary")

    var age: Int
        get() = ageDelegator.getMethod()
        set(value: Int) = ageDelegator.setMethod(value)
    var salary: Int
        get() = salaryDelegator.getMethod()
        set(value: Int) = salaryDelegator.setMethod(value)
}

fun main() {
    val p = Person("K")
    p.age = 21
    p.salary = 2100
    println("${p.name} - age: ${p.age}, salary: ${p.salary}")
}
```

```
age set: 21
salary set: 2100
age get: 21
salary get: 2100
K - age: 21, salary: 2100 
```

여기에 더하여 코틀린은 by를 지원하여 프로퍼티의 get과 set을 이어주도록 만들어준다.
실제 Property Delegation에서 제공하는 Delegator가 아래와 같이 만들어져 있다.

```kotlin
class Person(val name: String, age: Int, salary: Int) {
    var age: Int by Delegator(age
    var salary: Int by Delegator(salary)
}

class Delegator(var value: Int) {
    operator fun getValue(thisRef: Person, property: KProperty<*>): Int {
        println("${property.name} get: $value")
        return value
    }

    operator fun setValue(thisRef: Person, property: KProperty<*>, newValue: Int) {
        println("${property.name} set: $newValue")
        value = newValue
    }
}

fun main() {
    val p = Person("K", 20, 2000)
    p.age = 21
    p.salary = 2100
    println("${p.name} - age: ${p.age}, salary: ${p.salary} ")
}
```

```
age set: 21
salary set: 2100
age get: 21
salary get: 2100
K - age: 21, salary: 2100 
```

여기서 끝나지 않는다! 

해당 Delegater에서는 더 많은 기능 부여 할 수 있다.
아래는 실제 코틀린에서 제공하는 Delegator를 이용해 더 많은 기능을 부여해 보았다.


```kotlin
fun Person.makeDelegator(value: Int) = Delegator(this, value)

class Person(val name: String, age: Int, salary: Int) {
    val thisName = "Person 객체"

    var age: Int by makeDelegator(age)
    var salary: Int by makeDelegator(salary)
}

class Delegator(var person: Person, var value: Int) {

    init {
        println("Delegator 실행!! 여긴 ${person.thisName}")
    }
    operator fun getValue(thisRef: Person, property: KProperty<*>): Int {
        println("${property.name} get: $value")
        return value
    }

    operator fun setValue(thisRef: Person, property: KProperty<*>, newValue: Int) {
        println("${property.name} set: $newValue")
        value = newValue
    }
}

fun main() {
    val p = Person("K", 20, 2000)
    p.age = 21
    p.salary = 2100
    println("${p.name} - age: ${p.age}, salary: ${p.salary} ")
}
```

```
Delegator 실행!! 여긴 Person 객체
Delegator 실행!! 여긴 Person 객체
age set: 21
salary set: 2100
age get: 21
salary get: 2100
K - age: 21, salary: 2100 
```


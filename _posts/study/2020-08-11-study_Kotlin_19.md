---
layout: post
author: study
title:  "Kotlin 객체편 - [9]"
description: "내부 클래스"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 내부 클래스

코틀린의 내부 클래스 종류
- 중첩(Nested) 클래스
- 이너(Inner) 클래스


##### 자바의 내부클래스 종류

| 종류 | 역할 |
| --- | --- |
| 정적 클래스(static class) | static 키워드를 가지며 외부 클래스를 인스턴스화 하지않고 바로 사용가능한 클래스(주로 빌더 클래스에 이용) |
| 멤버 클래스(member class) | 인스턴스 클래스로도 불리며 외부 클래스의 필드나 메서드와 연동하는 내부 클래스 |
| 지역 클래스(local class) | 초기화 불록이나 메서드 내의 블록에서만 유효한 클래스 |
| 익명 클래스(anonymous class) | 이름이 없고 주로 일회용 객체를 인스턴스화 하면서 오버라이드 메서드를 구현하는 내부 클래스. 가독성이 떨어지는 단점이 있다. |

익명클래스는 하위클래스로 만들고 객체 생성하지 않아도 바로 인스턴스를 생성할수 있는데, 이를 코틀린에서는 Object가 할수 있다.


##### 자바와 코틀린의 내부 클래스 비교

| 자바 | 코틀린 |
| --- | --- |
| 정적 클래스(static class) | 중첩 클래스(nested class): 객체 생성 없이 사용 가능 |
| 멤버 클래스(member class) | 이너 클래스(inner class): 필드나 메서드와 연동하는 내부 클래스로 inner 키워드가 필요하다 |
| 지역 클래스(local class) | 지역 클래스(local class): 클래스의 선언이 블록에 있다면 지역 클래스이다. |
| 익명 클래스(anonymous class) | 익명 객체(ananymous object): 이름이 없고 주로 일회용 객체를 사용하기 위해 object 키워드를 통해 선언된다.|


```java
// 자바의 멤버(이너) 클래스
class A {
    ...
    class B {
        ... // 외부 클래스 A의 필드에 접근 가능
    }
}

// 자바의 정적 클래스
class C {
    
    static class D { // 정적 클래스를 위해 static 키워드 사용
        ...
    }
}
```

```javascript
// 코틀린의 이너 클래스 (자바의 멤버클래스와 동일!)
class A {
    ...
    inner class B{ // 자바와 달리 inner 키워드 필요
        ... // 외브 클래스 A의 필드에 접근 가능
    }
}

// 정적 클래스처럼 사용한 코틀린의 중첩 클래스
class C{
    
    class D { // 코틀린에서는 아무 키워드가 없는 클래스는 중첩 클래스이며
              // 정적 클래스처럼 사용한다.
        ...   // 외부 클래스 A의 프로퍼티, 메서드에 접근 할 수 없다.
    }
}
```

중첩 클래스(Nested class)는 코틀린에서 기본적으로 정적(static) 클래스 처럼 다뤄진다.
즉 정적클래스로 접근은 static이므로 가능하지만, 해당 정적클래스에서 외부 클래스로는 접근이 불가능하다!

```javascript
class Outer {
    val ov = 5
    class Nested {
        val nv = 10
        fun greeting() = "[Nested] Hello ! $nv" // 외부의 ov에는 접근 불가
    }

    fun outside() {
        val msg = Nested().greeting() // Outer 객체 생성 없이 중첩 클래스의 메서드 접근
        println("[Outer]: $msg, ${Nested().nv}") // 중첩 클래스의 프로퍼티 접근
    }
}

fun main() {
    //static 처럼 Outer의 객체 생성 없이 Nested객체를 생성 사용할 수 있음
    val output = Outer.Nested().greeting()
    println(output)

    // Outer.outside() // 에러! Outer 객체 생성 필요
    val outer = Outer()
    outer.outside()
}
```

중첩 클래스에서 바깥 클래스로 접근하는 방법
- Outer클래스가 컴패니언 객체를 가지고 있을 때 접근 가능하다

```javascript
class Outer {
    class Nested {
        ...
        fun accessOuter() { // 컴패니언 객체는 접근할 수 있음
            println(country)
            getSomething()
        }
    }
    companion object { // 컴패니언 객체는 static처럼 접근 가능
        const val country = "Korea"
        fun getSomething() = println("Get something...")
    }
}
```

```javascript
class Outer {
    val ov = 5
    class Nested {
        val nv = 10
        fun greeting() = "[Nested] Hello ! $nv" // 외부의 ov에는 접근 불가
        fun accessOuter() {
            println(country)
            getSomething()
        }
    }

    fun outside() {
        val msg = Nested().greeting() // Outer 객체 생성 없이 중첩 클래스의 메서드 접근
        println("[Outer]: $msg, ${Nested().nv}") // 중첩 클래스의 프로퍼티 접근
    }

    companion object {
        const val country = "Korea"
        fun getSomething() = println("Get something...")
    }
}

fun main() {
    //static 처럼 Outer의 객체 생성 없이 Nested객체를 생성 사용할 수 있음
    val output = Outer.Nested().greeting()
    println(output)
    Outer.Nested().accessOuter()
    // Outer.outside() // 에러! Outer 객체 생성 필요
    val outer = Outer()
    outer.outside()
}
```


코틀린의 이너 클래스
- 특별한 키워드인 inner를 사용해 선언된 내부 클래스
- 이너 클래스는 바깥 클래스의 멤버들에 접근 가능
- 바깥 클래스의 private 멤버도 접근이 가능


```javascript
class Smartphone(val model: String) {

    private val cpu = "Exynos"

    inner class ExternalStorage(val size: Int) {
        fun getInfo() = "${model}: Installed on $cpu with ${size}Gb" // 바깥 클래스의 프로퍼티 접근
    }
}

fun main() {
    val mySdcard = Smartphone("S7").ExternalStorage(32)
    println(mySdcard.getInfo())
}
```
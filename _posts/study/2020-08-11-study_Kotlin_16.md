---
layout: post
author: study
title:  "Kotlin 객체편 - [6]"
description: "정적 변수, 컴패니언 객체, 최상위 함수, object, 싱글톤"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 정적 변수와 컴페니언 객체

사용 범위에 따른 분류
- 지역(local)
- 전역(global)

보통 클래스는 동적으로 객체를 생성하는데 정적으로 고정하는 방법
- 동적인 초기화 없이 서용할 수 잇는 개념으로 자바에서는 static 변수 또는 객체가 존재하는데, 코틀린에서는 이것을 컴페니언 객체(Companion object)로 사용한다
- 프로그램 실행 시 고정적으로 가지는 메모리로 객체 생성 없이 사용할 수 있다.
- 단, 자주 사용되지 않는 변수나 객체를 만들게 되면 메모리 낭비에 대한 이슈가 존재한다.


```javascript
class Person {
    var id: int = 0
    var name: String = "Youngdeok"
    companion object { // 이러한 객체 즉, 컴패니언 객체는 실제 객체의 싱글톤(singleton)으로 정의된다.
        var language: String = "Korean"
        fun work() {
            println("working...")
        }
    }
}

fun main() {
    println(Person.language) // 인스턴스를 생성하지 않고 기본값 사용
    Person.language = "English" // 기본값 변경 가능
    println(Person.language) // 변경된 내용 출력
    Person.work() // 메서드 실행
    // println(Person.name) // name은 companion object가 아니므로 에러
}
```


코틀린에서 자바의 static 멤버의 사용
```java
// Customer.java
public class Customer {
    public static final String LEVEL = "BASIC"; // static 필드
    public static void login() { // static 메서드
        System.out.println("Login...");
    }
}
```

```javascript
// CustomerAccess.kt
// 코틀린에서 자바의 static 접근
fun main() {
    println(Customer.LEVEL)
    Customer.login()
}
```

자바에서 코틀린 컴패니언 객체 사용

@JvmStatic 애노테이션 표기법 사용

```javascript
class KCustomer {
    companioon object {
        const val LEVEL = "INTERMEDIATE"
        @JvmStatic fun login() = println("Login...")  // 애노테이션 표기 사용
    }
}
```

```java
public class KCustomerAccess {
    public static void main(String[] args) }
    // 코틀린 클래스의 컴패니언 객체를 접근
    System.out.println(KCustomer.LEVEL);
    KCustomer.login(); // 애노테이션을 사용할 떄 접근 방법
    KCustomer.Companion.login(); // 위와 동일한 결과로 애노테이션을 사용하지 않을 때 접근 방법
}
```


### 최상위 함수(top-level function)

최상위 함수
- 클래스 없이 만들었던 최상위 함수들은 객체 생성 없이도 어디엣든 실행
- 패키지 레벨 함수(package-level function) 라고도 함
- 최상위 함수는 결국 자바에서 static final로 선언된 함수이다.

자바에서 코틀린의 최상위 함수 접근
- 코틀린의 최상위 함수는 클래스가 없으나 자바와 연동시 내부적으로 파일명에 Kt 접미사가 붙은 클래스를 자동 생성하게 된다.
- 자동 변환되는 클래스명을 명시적으로 지정하고자 하는 경우 @file:JvmName("ClassName")을 코드 상단에 명시한다.


```javascript
// PackageLevelFUnction.kt
fun packageLevelFUnc() {
    println("Package-Level Function")
}

fun main() {
    packageLevelFunc()
}
```

```java
//PackageLevelAccess.java
public class PackageLevelAccess {
    public static void main(String[] args) {
        PackageLevelFunctionKt.packageLevelFunc();
    }
}
```

이름을 명시하여 최상위 함수 접근
```javascript
// PackageLevelFUnction.kt

@file:JvmName("PKLevel")
fun packageLevelFUnc() {
    println("Package-Level Function")
}

fun main() {
    packageLevelFunc()
}
```

```java
//PackageLevelAccess.java
public class PackageLevelAccess {
    public static void main(String[] args) {
        PKLevel.packageLevelFunc();
    }
}
```


### object와 싱글톤
상속할 수 없는 클래스에서 내용이 변경된 객체를 생성할 때
- 자바의 경우 익명 내부 클래스를 사용해 새로운 클래스 선언
- 코틀린에서는 object 표현식이나 object 선언으로 이 경우를 좀더 쉽게 처리한다


```javascript
object OCustomer {
    var name = "Kildong"
    fun greeting() = println("Hello World!")
    val HOBBY = Hobby("Basketball")
    init {
        println("Init!")
    }
}
...

class Hobby(val name: String)

fun main() {
    OCustomer.greeting() // 객체의 접근 시점
    OCustomer.name = "Dooly"
    println("name = ${OCustomer.name}")
    println(OCustomer.HOBBY.name)
    ...
}
```

object 선언 방식은 접근 시점에 객체가 생성된다.
그렇기 때문에 생성자 호출을 하지 않으므로 object 선언에는 주/부 생성자를 사용할 수 없다.

자바에서는 `OCustomer.INSTANCE.getName();` 와 같이 접근 해야 한다.


### object 표현식

object: - object 표현식을 사용할 때
- object 선언과 달리 이름이 없으며 싱글톤이 아니다.
- 따라서 object 표현식이 사용될 때마다 새로운 인스턴스가 생성된다.
- 이름이 없는 익명 내부 클래스로 불리는 형태를 object 표현식으로 만들수 있다.


```javascript
open class Superman() {
    fun work() = println("Taking photos")
    fun talk() = println("Talking with people.")
    open fun fly() = println("Flying in the air.")
}

fun main() {
    val pretendedMan = object: Superman() { // object 표현식으로 fly() 구현의 재정의
        override fun fly() = println("I'm not a real superman. I can't fly!")
    }
    pretendedMan.work()
    pretendedMan.talk()
    pretendedMan.fly()
}
```
---
layout: post
author: study
title:  "Kotlin - by 2. Implementation by Delegation"
description: "코틀린의 Implementation by Delegation 동작 분석"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


### 개요

지난 포스팅과는 다른 기능을 하는 by를 분석하려고 한다.


### Implementation by Delegation

[kotlin에서 설명하는 Implementation by Delegation](https://kotlinlang.org/docs/reference/delegation.html)의 예시 프로그램을 이용하여 by Delegation을 이해해보자.

```kotlin
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

class Derived(b: Base) : Base by b

fun main() {
    val b = BaseImpl(10)
    Derived(b).print()
}
```

```
10
```

우선 레퍼런스는 위의 예시에 대해 아래와 같이 설명한다.

`상속을 표현하는 슈퍼타입 리스트 내의 by 절은 b(에 대한 참조)가 상속 오브젝트의 내부에 저장되고 컴파일러가 b가 가지는 Base 인터페이스의 모든 메소드를 생성함을 나타냅니다`


그렇다면 여기서 

```kotlin
class Derived(b: Base) : Base by b
```

이 부분을 디컴파일 해보면 다음과 같다. (그 윗부분은 알고있는 상속 인터페이스)

```java
public final class Derived implements Base {
   // $FF: synthetic field
   private final Base $$delegate_0;

   public Derived(@NotNull Base b) {
      Intrinsics.checkParameterIsNotNull(b, "b");
      super();
      this.$$delegate_0 = b;
   }

   public void printX() {
      this.$$delegate_0.printX();
   }
}
```

&#36;&#36;delegate_0가 Base 타입의 본래 인스턴스를 참조할 수 있도록 생성되며, printX()도 정적 메소드로 생성되어 &#36;&#36;delegate_0의 printX()를 호출할 수 있도록 생성된다. 

그렇기 때문에, Derived를 사용할 때 Base에 대한 명시적 참조를 생략하고, printX() 메소드를 호출하는 것이 가능하다.


그렇다면 오버라이딩 한 멤버를 위임할 경우는 어떻게 될까?

```kotlin
interface Base {
    fun printMessage()
    fun printMessageLine()
}

class BaseImpl(val x: Int) : Base {
    override fun printMessage() { print(x) }
    override fun printMessageLine() { println(x) }
}

class Derived(b: Base) : Base by b {
    override fun printMessage() { print("abc") }
}

fun main() {
    val b = BaseImpl(10)
    Derived(b).printMessage()
    Derived(b).printMessageLine()
}
```

```
abc10
```

다시 디컴파일을 해보자

```java
public final class Derived implements Base {
   // $FF: synthetic field
   private final Base $$delegate_0;

   public void printMessage() {
      String var1 = "abc";
      boolean var2 = false;
      System.out.print(var1);
   }

   public Derived(@NotNull Base b) {
      Intrinsics.checkParameterIsNotNull(b, "b");
      super();
      this.$$delegate_0 = b;
   }

   public void printMessageLine() {
      this.$$delegate_0.printMessageLine();
   }
}
```

종합적으로 .. 상속과 동일하다!?

base를 impl한 b와 base를 impl한 derived는 상속 관계가 된다.

사실 기본적으로 코틀린 클래스는 JVM의 final속성을 가지고 있기 때문에, 상속을 하려면 open 키워드를 사용해야 한다. 그런데 위임을 사용한다면 레퍼런스에서 소개한대로 상속에 대안이 될수가 있다.

`The Delegation pattern has proven to be a good alternative to implementation inheritance, and Kotlin supports it natively requiring zero boilerplate code`


이번에 다른 예제를 살펴보자.

CoffeeMaker 프로그램이다.

```kotlin
interface Heater {
    fun on()
    fun off()
    fun isHot() : Boolean
}

class ElectricHeater(var heating: Boolean = false) : Heater {
    override fun on() {
        println("~ ~ ~ heating ~ ~ ~")
        heating = true
    }

    override fun off() {
        heating = false
    }

    override fun isHot() : Boolean {
        return heating
    }
}

interface Pump {
    fun pump()
}

class Thermosiphon(heater: Heater) : Pump, Heater by heater {
    override fun pump() {
        if (isHot()) {
            println("=> => pumping => =>");
        }
    }
}

interface CoffeeModule {
    fun getThermosiphon() : Thermosiphon
}

class MyDripCoffeeModule : CoffeeModule {
    companion object {
        val electricHeater: ElectricHeater by lazy {
            ElectricHeater()
        }
    }

    private val _thermosiphon : Thermosiphon by lazy {
        Thermosiphon(electricHeater)
    }
    
    override fun getThermosiphon() : Thermosiphon = _thermosiphon
}

class CoffeeMaker(val coffeeModule: CoffeeModule) {
    fun brew() {
        coffeeModule.getThermosiphon().run {
            on() // heater.on 이 아니다!
            pump()
            println(" [_]P coffee! [_]P ")
            off() // heater.off 가 아니다!
        }
    }
}

fun main(args: Array<String>) {
    val coffeeMaker = CoffeeMaker(MyDripCoffeeModule())
    coffeeMaker.brew();
}
```

[위 글은 이 포스트에서 참고하였습니다.](https://medium.com/til-kotlin-ko/kotlin%EC%9D%98-%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%9C%84%EC%9E%84%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80-c14dcbbb08ad)
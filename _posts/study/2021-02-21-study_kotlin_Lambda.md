---
layout: post
author: study
title:  "Functional Interface - Lambda"
description: "Functional Interface는 또 뭐야?"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---

### 개요

옵저버 패턴을 다뤘던 이전 포스팅에서 liveData를 최대한 비슷하게 구현해보고자 하는 과정에서 아래와 같은 이슈가 발생했다.

> `코틀린 interface를 매개로 사용하는 함수에 대해 람다식을 적용이 되지 않는다.`
 
이전 포스팅에서는 해당 이슈에 대해 람다식을 적용하지 않고 익명 객체로 처리를 했지만, 더욱 깔끔한 코딩을 위해 람다식을 적용하고자 한다면 아래와 같은 2가지 해결방안이 존재한다.

1. 해당 interface를 java로 만든다.

```java
public interface MyObserver {
    void update(String title, String news);
}
```

2. Kotlin의 버전이 1.4 이상인 버전에 한해서 interface 앞에 fun 키워드를 붙여준다.

```kotlin
fun interface MyObserver {
    fun update(title: String, news: String)
}
```

이 포스팅은 여기서 부터 출발 한다.

<Br>

### Functional Interface - Lambda Expression

코틀린이 나온 결정적인 이유는 `자바로 람다식 쓰고싶어서`라고 생각한다. 

그러자 자바도 람다식을 쓰고 싶어서 고민하기 시작했고, 그 결과 제한적이지만 람다식을 사용할수 있게 되었다.
어떻게 가능하게 되었는지, 어떤 제한이 있는지를 살펴보자.

[우선 Java Specification에선 람다식에 대해 다음과 같이 설명하고 있다.](https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.27)

>A lambda expression is like a method: it provides a list of formal parameters and a body - an expression or block - expressed in terms of those parameters. 
>
>람다식은 메소드와 비슷하다: 규격을 갖춘 파라미터의 목록과 그 파라미터를 이용해서 표현된 몸통(표현식이든 블록이든)을 제공한다.
>
>(중간 생략)
>
>Evaluation of a lambda expression produces an instance of a functional interface (§9.8). Lambda expression evaluation does not cause the execution of the expression's body; instead, this may occur at a later time when an appropriate method of the functional interface is invoked. 
>
>람다식이 평가(evaluation)되면 그 결과 Functional Interface의 인스턴스를 생성한다. 람다식의 처리 결과는 표현식 몸통을 실행하는 것이 아니다; 대신 나중에 이 Functional Interface의 적절한 메소드가 실제 호출(invoke)될 때 이것(표현식 몸통의 실행)이 일어난다.
>
>(중간 생략)

Functional Interface는 무엇일까?

Functional Interface는 일반적으로 '구현해야 할 추상 메소드가 하나만 정의된 인터페이스'를 가리킨다. 

Java Language Specification의 설명을 인용하면 다음과 같다.

>A functional interface is an interface that has just one abstract method (aside from the methods of Object), and thus represents a single function contract.
>unctional Interface는 (Object 클래스의 메소드를 제외하고) 단 하나의 추상 메소드만을 가진 인터페이스를 의미하며, 그런 이유로 단 하나의 기능적 계약을 표상하게 된다.

아래의 예시를 통해 간단하게 설명을 해보자면,

```java
public interface MyObserver {
    void update(String title, String news);
}
```

```kotlin
fun NewsMachine.add(update: (title:String , news: String) -> Unit ): MyObserver {
    val wrappedObserver = object : MyObserver {
        override fun update(title: String, news: String) {
            update.invoke(title, news)
        }
    }
    add(wrappedObserver)
    return wrappedObserver
}
```

Java의 특정한 조건을 갖춘 Interface인 Functional Interface라고 하는데, 람다식의 평가 결과가 해당 Interface의 인스턴스가 된다. 따라서 익명 객체에 Interface를 implement한 동작을 아래와 같이 람다식으로 바꿀수가 있다.

```kotlin
fun NewsMachine.add(update: (title:String , news: String) -> Unit ): MyObserver {
    val wrappedObserver = MyObserver { title, news -> update.invoke(title, news) }
    add(wrappedObserver)
    return wrappedObserver
}
```


그렇게 되면 코드가 더욱 깔끔해진다.


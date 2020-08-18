---
layout: post
author: study
title:  "Kotlin 객체편 - [14]"
description: "Kotlin 자료형 프로젝션"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 자료형 프로젝션
선언 지점 변성(declaration-site variance)
- 클래스 자체에 가변성을 지정하는 방식으로 클래스에 in/out을 지정할 수 있다.
- 선언하면서 지정하면 클래스의 공변성을 전체적으로 지정하는 것
    - 클래스를 사용하는 장소에서는 따로 자료형을 지정해 줄 필요가 없음

```javascript
class <Box in T: Animal>(var item: T)
```

사용지점 변성(use-site variance)
- 매서드 매개변수에서 똗는 제네릭 클래스를 생성할 때와 같이 사용 위치에서 가변성을 지정하는 방식

사용지점 변성의 예
```javascript
class Box<T>(var item: T)
```

Box의 사용시점에서 box의 item을 얻느냐(get) 설정하느냐(set)에 따라 out/in 결정

```javascript
fun <T> printObj(box: Box<out Animal>) {
    val obj: Animal = box.item // item의 값을 얻음(get)
    // box.item = Animal() // Error! 설정(set)하려고 할때는 in이 지정되어야 한다.
    println(obj)
}
```

이러한 방법을 자료형 프로젝션이라고 하며, 지료형 프로젝션을 통해 자료의 안정성을 보장한다.

### 스타 프로젝션

in/out을 정하지 않고 추후에 결정
- 어떤 자료형이라도 들어올 수 있으나 구체적으로 자료형이 결정되고 난 후에는 그 자료형과 하위 자료형의 요소만 담을 수 있도록 제한

Foo<out T: TUpper>
- Foo<*>는 Foo<out TUpper>와 동일

Foo<in T>
- Foo<*>는 Foo<in Nothing>와 동일


```javascript
class InOutTesT<in T, out U>(t: T, u: U) {
    // val propT: T = t // Error! T는 in 위치기 떄문에 out위치에 사용 불가
    val propU: U = u // U는 out위치로 사용 가능

    //fun func1(u: U) // Error! U는 out 위치기 때문에 in위치에 사용 불가
    fun func2(t: T) { // T는 in 위치에 사용 가능
        print(t)
    }
}

fun starTestFunc(v: InOutTest<*, *>) {
    // v.func2(1) // Error: Nothing으로 인자를 처리함
    print(v.propU)
}
```

| 종류 | 예 | 가변성 | 제한 |
| --- | --- | --- | --- |
| out 프로젝션 | Box<out Cat> | 공변성 | 형식 매개변수는 세터를 통해 값을 설정하는 것이 제한됨 |
| in 프로젝션 | Box<in Cat> | 반공변성 | 형식 매개변수는 게터를 통해 값을 읽거나 반환 가능 |
| 스타 프로젝션 | Box<*> | 모든 인스턴스는 하위타입이 될 수 있음 | in과 out은 사용 방법에 따라 결정됨 |


### reified 자료형

reified 자료형이 필요한 이유
```javascript
fun< T> myGenericFun(c: Class<T>)
```
- T 자료형은 실행 시간에 삭제
- 컴파일 시간에는 접근 가능하나 함수 내부에서 사용하려면 위의 코드에서 함수의 매개변수를 넣어 c: Class<T> 처럼 지정해야만 실행 시간에 사라지지 않고 접근 가능하다.

```javascript
inline fun <reified T> myGenericFun()
```
- reified로 형식 매개변수 T를 지정하면 실행 시간에 접근 가능
- reified 자료형은 inline 함수에서만 사용할 수 있다.
    - 컴파일러가 복사해 넣을 때 실제 자료형을 알 수 있기 때문에 실행 시간에도 사용할 수 있게 됨

Class<T>
- .class 형태로 반환받는 객체
- Class라는 클래스는 원본 클래스에 대한 많은 메타 데이터를 가진다
    - 패키지 이름이나 메서드, 필드, 구현된 인터펭스, 각종 검사 변수 등

Object::class
- 코틀린의 표현 방법으로 KClass를 나타낸다

자바의 클래스를 가져오려면 .java를 사용해야 한다
```
Object::class // KClass
Object::class.java // Class
```


reified를 이용한 결정됮 않은 제네릭 자료형의 처리
```java
fun main() {
    val result = getType<Float>(10)
    println("result = $result")
}

inline fun <reified T> getType(value: Int): T {
    println(T::class) // 실행 시간에 삭제되지 않고 사용 가능
    println(T::class.java)

    return when (T::class) { // 받아들인 제네릭 자료형에 따라 반환
        Float::class => value.toFloat() as T
        Int::class -> value as T 
        /*
        as는 보통 임의로 캐스팅 하기 위해 사용.
        여기서는 T형식으로 캐스팅 하는데는
        '안전하지 않은' 캐스팅이로도 불리고
        '안전한' 캐스팅을 하려면 as? 와 같이 사용한다
        as?는 캐스팅 불가 시 null을 반환한다.
        */
        else -> throw IllegalStateException("${T::class} is not supported!")
    }
}
```
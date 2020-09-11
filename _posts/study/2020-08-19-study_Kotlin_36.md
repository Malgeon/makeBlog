---
layout: post
author: study
title:  "Kotlin 객체편 - [26]"
description: "코루틴 - [1]"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 코루틴
개념
- 스레드와 달리 코틀린은 코루틴을 통해 복잡성을 줄이고도 손쉽게 일시 중단하거나 다시 시작하는 루틴을 만들어 낼 수 있다.
- 멀티태스킹을 실현하면서 가벼운 스레드라고도 불림
- 코루틴은 문맥 교환 없이 해당 루틴을 일시 중단(suspended)을 통해 제어 


### 코루틴의 주요 패키지

- kotlinx.coroutines의 common 패키지
| 기능 | 설명 |
| --- | --- |
| launch / async | 코루틴 빌더 |
| Job / Deferred | cancellation 지원을 위한 기능 |
| Dispatchers | Default는 백그라운드 코루틴읠 위한 것이고 Main은 Android나 Swing, JavaFx를 위해 사용 |
| delay / yield | 상위 레벨 지연(suspending) 함수 |
| Channel / Mutex | 토신과 동기화를 위한 기능 |
| coroutineScope / supervisorScope | 범위 빌더 |
| select | 표현식 지원 |

- core 패키지
| 기능 | 설명 |
| --- | --- |
| CommonPool | 코루틴 컨텍스트 |
| produce / actor | 코루틴 빌더 |



### 코루틴 빌더
launch
- 일단 실행하고 잊어버리는(fire-and-forget) 형태의 코루틴으로 메인 프로그램과 독립하여 실행할 수 있다.
- 기본적으로 즉시 실행하며 블록 내의 실행 결과는 반환하지 않는다.
- 상위 코드를 블록 시키지 않고(넌블로킹) 관리를 위한 Job 객체를 즉시 반환한다.
- join을 통해 상위 코드가 종료되지 않고 완료를 기다리게 할 수 있다.

async
- 비동기 호출을 위해 만든 코루틴으로 결과나 예외를 반환한다.
- 실행 결과는 Deffered<T>를 통해 반환하며 await를 통해 받을 수 있다.
- await는 작업이 완료될 때까지 기다리게 된다.


practice

우선 코루틴 라이브러리를 사용하기 위해선 Project Structure에 들어야 한다.
intelliJ - File - Project Structure (Ctrl + Alt + Shift + S)

좌측 배너에서 Libraries에 들어가서 라이브러리를 추가해 주어야 하는데, 기본 설정 방법인 Maven을 이용해 추가를 할 수있다.(Gradle도 가능하다.)

중간 배너에서 +를 클릭하고, Maven을 클릭한 뒤 coroutine을 검색할 수 있다.
검색 결과중 가장 최신 라이브러리를 가지고 오면 된다.

```java
import kotlinx.coroutines.*
// 1번 코루틴
fun main() { // 메인 스레드의 문맥 
    /*-------------------------------------------------------------*/
    // 2번 코루틴
    GlobalScope.launch { // 새로운 코루틴을 백그라운드에 실행
        delay(1000L) // 1초의 넌블로킹 지연 (시간의 기본단위는 ms)
        println("World!") // 지연 후 출력
    }
    /*--------------------------------------------------------------*/
    println("Hello,") // main 스레드가 코루틴이 지연되는 동안 계속 실행
    Thread.sleep(2000L) // main 스레드가 JVM에서 바로 종료되지 않게 2초 기다림
}
```

일시 중단(suspended)함수
- delay()의 경우 일시 중단될 수 있으며 필요한 경우 재개(resume)된다.
    - 이 소스의 경우 약 1초 후


delay()의 선언부
```java
// kotlinx-coroutines-core의 DelayKt.class의 일부
public suspend fun delay(timeMillis: kotlin.Long) kotlin.Unit { 
    /* compiled code */ }
```
- suspend 함수를 코루틴 블록 외에 사용하면 오류가 난다.

사용자 함수에 suspend적용
```java
suspend fun doSomething() {
    println("Do something!")
}
```
- 컴파일러는 suspend가 붙은 함수를 자동적으로 추출해 Continuation 클래스로부터 분리된 루틴을 만든다.
- 이러한 함수를 사용하기 위해 코루틴 빌더인 launch와 async에서 이용할 수 있다.



practice
```java
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch() {
        delay( 1000L )
        println("World")
    }

    println("Hello")
}
```
```
Hello
```
코루틴이 돌고 있다 하더라도 println("Hello")의 동작을 마치면 함수가 끝나므로 World를 출력하지 않는다.


```java
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch() {
        delay( 1000L )
        println("World")
    }
    println("Hello")
    THread.sleep( 2000L )
}
```
```
Hello
World
```

```java
import kotlinx.coroutines.*

fun main() {
    //doSomething() // Error! suspend 함수는 coroutine 안에서만 사용 가능!
    GlobalScope.launch() {
        delay( 1000L )
        println("World")
        doSomething()
    }
    println("Hello")
    THread.sleep( 2000L )
}

suspend fun doSomething() {
    println("Do Something")
}
```
```
Hello
World
Do Something
```
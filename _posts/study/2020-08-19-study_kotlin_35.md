---
layout: post
author: study
title:  "Kotlin 객체편 - [25]"
description: "Kotlin 동시성 프로그래밍"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 동시성 프로그래밍

동기적(synchronous) 수행
- 순서대로 작업을 수행하여 하나의 루틴을 완료한 후 다른 루틴을 실행하는 방식
- 다양한 기능이 한꺼번에 일어나는 다중 실행 환경에서는 성능상의 제약 발생
    - 예시) UI, 데이터 다운로드를 동시에 대응해야 하는 경우

비동기적(asynchronous) 수행
- 다양한 기능을 동시에 수행할 수 있는 방식
- 전통적인 스레드를 이용하거나 RxJava, Reactive와 같은 서드파티(third-party)라이브러리에서 제공
- 코틀린에서는 코루틴 (coroutines)을 기본으로 제공


코루틴
- 먼저 하나의 개별적인 작업을 루틴(routine)이라고 부르는데 여러 개의 루틴들이 협력(co)한다는 의미로 만들어진 합성어
- 코틀린의 코루틴을 사용하면 넌블로킹(non-blocking) 또는 비동기 코드를 마치 일반적인 동기 코드처럼 쉽게 작성하면서도 비동기 효과를 낼 수 있다.

![blocking]({{ site.baseurl }}/assets/images/study/kotlin/kotlin_oop_blocking.JPG)

![nonblocking]({{ site.baseurl }}/assets/images/study/kotlin/kotlin_oop_nonblocking.JPG)

Concurrency(병행성) : 시간을 짧게 나눠 하나의 코어에서 실행 - OS의 스케쥴러에서 처리를 해준다.


### 프로세스와 스레드

태스크(Task) 개념
- 보통 태스크는 큰 실행 단위인 프로세스(process)나 조금 더 작은 실행 단위인 스레드(thread)로 생각할 수 있다.
- 프로세스는 실행되는 메모리, 스택, 열린 파일 등을 모두 포함하기 때문에 프로세스간 문맥 교환(context-switching)을 하는 데 비용이 크다.
- 스레드는 자신의 스택만 독립적으로 가지고 나머지는 대부분 공유하므로 문맥 교환 비용이 낮아 프로그래밍에서 많이 사용된다.
- 다만 여러 개의 스레드를 구성하면 코드가 복잡하다.


문맥 교환(context-switching)
- 하나의 프로세스나 스레드가 CPU를 사용하고 있는 상태에서 다른 프로세스나 스레드가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태(문맥)를 보관하고 새로운 프로세스의 상태를 적재하는 과정
(코루틴 자체에서는 문맥 교환이 없다고 할 수 있다.)

![context-switching]({{ site.baseurl }}/assets/images/study/kotlin/kotlin_oop_context-switching.JPG)


스레드 연습
```java
// 클래스
class SimpleThread : Thread() {
    override fun run() {
        println("Class Thread ${Thread.currentThread()}")
    }
}
// 인터페이스
class SimpleRunnalbe : Runnable {
    override fun run() {
        println("Interface Thread ${Thread.currentThread()}")
    }
}
fun main() {
    val thread = SimpleThread()
    thread.start()

    val runnalbe = SimpleRunnable()
    val thread2 = Thread(runnable)
    thread2.start()

    // 익명 객체
    object : Thread() {
        override fun run() {
            println("object Thread: ${Thread.currentThread()}")
        }
    }.start()

    // 람다식
    Thread {
        println("Lambda Thread: ${Thread.currentThread()}")})
    }.start()
}
```

```java
// 람다식 추가 함수를 만들어 실행
public fun thread(start: Boolean = true, isDaemon: Boolean = false,
                  contextClassLoader: ClassLoader? = null, name: String? = null,
                  priority: Int = -1, block: () -> Unit): Thread {
    
    val thread = obejct : Thread() {
        public override fun run() {
            block()
        }
    }

    if (isDaemon) thread.isDaemon = true // 백그라운드 실행 여부
    if (priority > 0) thread.priority = priority // 우선순위 ( 1: 낮음 ~ 5: 보통 ~ 10: 높음) [default는 5]
    if (name != null) thread.name = name // 이름
    if (contextClassLoader != null) thread.contextClassLoader = contextClassLoader
    if (start) thread.start()

    return thread
}

fun main() {
    // 스레드의 옵션 변수를 손쉽게 설정할 수 있다.

    thread(start = true) {
        println("Current Threads(Custom function): ${Thread.currentThread()}")
        println("Priority: ${Thread.currentThread().priority}") // 기본값은 5
        println("Name: ${Thread.currentThread().name}")
        println("Name: ${Thread.currentThread().isDaemon}")
    }
}
```
```
Current Threads(Custom function): Thread[Thread-0,5,main]
Priority: 5
Name: Thread-0
Name: false
```

### 스레드 풀 사용하기
newFixedThreadPool()
- 자주 재사용되는 스레드를 이요하기 위해 미리 생성된 스레드풀에서 스레드 이용
- 예시) 8개의 스레드로 특정 백그라운드 서비스를 ㅏ도록 만든다고 했을 때

```java
val myService:ExecutorService = Executors.newFixedThreadPool(8)
var i = 0

while (i < items.size) { // 아주 큰 데이터를 처리할 때
    val item = items[i]
    myService.submit {
        processItem(item) // 여기서 아주 긴 시간 동안 처리 하는 경우
    }

    i += 1
}
```
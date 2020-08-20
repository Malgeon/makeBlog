---
layout: post
author: study
title:  "Kotlin 객체편 - [28]"
description: "Kotlin 코루틴 - 순차실행과 비동기"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

```java
import kotlinx.coroutines.*

suspend fun doWork1(): String {
    delay( 1000 )
    return "Work1"
}

suspend fun doWork2(): String {
    delay( 3000 )
    return "Work2"
}

private fun worksInSerial() {
    // 순차적 실행
    GlobalScope.launch {
        val one = doWork1()
        val two = doWork2()
        println("Kotiln One : $one")
        println("Kotiln two : $two")
    }
}

fun main() {
    worksInSerial()
    readLine()
}
```
```
Kotlin One : Work1
Kotlin Two : Work2
```


```java
import kotlinx.coroutines.*

suspend fun doWork1(): String {
    delay( 3000 )
    return "Work1"
}

suspend fun doWork2(): String {
    delay( 1000 ) // 시간을 반대로 하더라도 같은 결과
    return "Work2"
}

private fun worksInSerial() {
    // 순차적 실행
    GlobalScope.launch {
        val one = doWork1()
        val two = doWork2()
        println("Kotiln One : $one")
        println("Kotiln two : $two")
    }
}

fun main() {
    worksInSerial()
    readLine() // Enter를 눌러서 종료되게끔 하는 장치
}
```
```
Kotlin One : Work1
Kotlin Two : Work2
```


```java
import kotlinx.coroutines.*

suspend fun doWork1(): String {
    delay( 3000 )
    return "Work1"
}

suspend fun doWork2(): String {
    delay( 1000 ) // 시간을 반대로 하더라도 같은 결과
    return "Work2"
}

private fun worksInSerial(): Job {
    // 순차적 실행
    val job = GlobalScope.launch {
        val one = doWork1()
        val two = doWork2()
        println("Kotiln One : $one")
        println("Kotiln two : $two")
    }

    return job
}

fun main() = runBlocking {
    val time = measureTimeMillis {
        val job = worksInSerial()
        job.join()
    }
    println("time: $time")
}
```
```
Kotlin One : Work1
Kotlin Two : Work2
time: 4026 // 병행적으로 실행되지 않고 순차적임을 알수 있다.
```


### async 코루틴 빌더 생성

동시성 처리를 위한 async 코루틴
- launch와 다른 점은 Deferred<T>를 통해 결과값을 반환
- 지연된 결과 값을 받기 위해 await()를 사용

```java
...
private fun worksInParallel() {
    // Deferred<T> 를 통해 결과값을 반환
    // 새로운 코루틴 빌더 (병행 처리!)
    /*------------------------------------------------*/
    val one = GlobalScope.async {
        doWork1()
    }
    /*------------------------------------------------*/
    // 새로운 코루틴 빌더 (병행 처리!)
    /*------------------------------------------------*/
    val two = GlobalScope.async {
        doWork2()
    }
    /*------------------------------------------------*/
    // await에서 대기!
    /*------------------------------------------------*/
    GlobalScope.launch {
        val combined = one.await() + "_" + two.await()
        println("Kotlin Combined : $combined")
    }
    /*------------------------------------------------*/
}
...
```


```java
import kotlinx.coroutines.*

suspend fun doWork1(): String {
    delay( 3000 )
    return "Work1"
}

suspend fun doWork2(): String {
    delay( 1000 ) // 시간을 반대로 하더라도 같은 결과
    return "Work2"
}

private fun worksInSerial(): Job {
    // 순차적 실행
    val job = GlobalScope.launch {
        val one = doWork1()
        val two = doWork2()
        println("Kotiln One : $one")
        println("Kotiln two : $two")
    }

    return job
}

private fun worksInParallel(): Job {
    // 순차적 실행
    val one = GlobalScope.async {
        doWork1()
    }
    val two = GlobalScope.async {
        doWork2()
    }

    val job = GlobalScope.launch {
        val combined = one.await() + "_" + two.await()
        println("Kotlin Combined: $combined")

    }

    return job
}

fun main() = runBlocking {
    val time = measureTimeMillis {
        val job = worksInParallel()
        job.join()
    }
    println("time: $time")
}
```
```
Kotlin One : Work1
Kotlin Two : Work2
time: 3026 // 병행적으로 실행 하고 있음을 할 수 있다.
```

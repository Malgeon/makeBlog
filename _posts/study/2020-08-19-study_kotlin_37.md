---
layout: post
author: study
title:  "Kotlin 객체편 - [27]"
description: "코루틴 - [2] [job]"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### Job()

```java
import kotlinx.coroutines.*

fun main() {
    val job = GlobalScope.launch() {
        delay( 1000L )
        println("World")
        doSomething()
    }
    println("Hello")
    println("job: $job")
    THread.sleep( 2000L )
    println("job: $job")
}

suspend fun doSomething() {
    println("Do Something")
}
```
```
Hello
job: StandaloneCoroutine{Active}@....
World
Do Something
job: StandaloneCoroutine{Completed}@....
```

```java
import kotlinx.coroutines.*

fun main() {
    val job = GlobalScope.launch() {
        delay( 1000L )
        println("World")
        doSomething()
    }
    println("Hello")
    println("job: ${job.isActive}, ${job.isCompleted}")
    THread.sleep( 2000L )
    println("job: ${job.isActive}, ${job.isCompleted}")
}

suspend fun doSomething() {
    println("Do Something")
}
```
```
Hello
job: true, false
World
Do Something
job: false, true
```


### Job 객체
Job
- 코루틴의 생명주기를 관리하며 생성된 코루틴 작업들은 부모-자식과 같은 관계를 가질 수 있다.

Job 규칙
- 부모가 취소(cancel)되거나 실행 실패하면 그 하위 자식들은 모두 취소된다.
- 자식의 실패는 그 부모에 전달되며 부모 또한 실패한다. (다른 모든 자식도 취소됨)

SupervisorJob
- 자식의 실패가 그 부모나 다른 자식에 전달되지 않으므로 실행을 유지할 수 있다.

#### Join() 결과 기다리기
Join()
- Job 객체의 join()을 사용해 완료를 기다릴 수 있다.
    - launch에서 반환 값을 받으면 Job 객체가 되기 때문에 이것을 이용해 main 메서드에서 join()을 호출할 수 있다.

```java
...
fun main() = runBlocking<Unit> {
    val job = launch { // Job의 객체를 반환
        delay(1000L)
        println("World!")
    }
    println("Hello")
    job.join() // 명시적으로 코루틴이 완료되길 기다림. 취소할 경우 job.cancel()을 사용
}
```


```java
import kotlinx.coroutines.*

fun main() {
    runBlocking { // coroutine Block 생성!
        val job = GlobalScope.launch() {
            delay( 1000L )
            println("World")
            doSomething()
        }
        println("Hello")
        println("job: ${job.isActive}, ${job.isCompleted}")
        job.join() // launch가 완료될 때까지 상위 루틴은 블록킹한다.
        // join()이 suspend 함수라서 coroutine 안에서만 사용가능
        println("job: ${job.isActive}, ${job.isCompleted}")
    }
}

suspend fun doSomething() {
    println("Do Something")
}
```
```
Hello
job: true, false
World
Do Something
job: false, true
```


### Job의 상태
| 상태 | isActive | isCompleted | isCancelled |
| --- | --- | --- | --- |
| New | false | false | false |
| Active(기본값 상태) | true | false | false |
| Completing | true | false | false |
| Cancelling | false | false | true |
| Cancelled(최종 상태) | false | true | true |
| Completed(최종 상태) | false | true | false |

![Job-Status]({{ site.baseurl }}/assets/images/study/kotlin/kotlin_oop_job_status.JPG)


### 코루틴의 중단과 취소

중단 (코루틴 코드 내에서)
- delay(시간값) - 일정 시간을 지연하며 중단
- yield() - 특정 값을 산출하기 위해 중단

취소 (코루틴 외부에서)
- Job.cancel() - 지정된 코루틴 작업을 즉시 취소
- Job.cancelAndJoin() - 지정된 코루틴 작업을 취소 (완료시까지 기다림)
- 기본적으로 부모 자식 관계에 적용될 수 있으며 부모 블록이 취소되면 모든 자식 코루틴이 취소된다.
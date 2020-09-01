---
layout: post
author: study
title:  "Kotlin 객체편 - [29]"
description: "코루틴 - 문맥과 스코프"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 코루틴 문맥

Coroutine Context
- 코루틴을 실행하기 위한 다양한 설정값을 가진 관리 정보
    - 코루틴 이름, 디스패처, 작업 상세사항, 예외 핸들러 등
- 디스패처는 코루틴 문맥을 보고 어떤 스레드에서 실행되고 있는지 식별이 가능해진다.
- 코루틴 문맥은 + 연산을 통해 조합될 수 있다.

```java
val comCoroutineContext = someJob + Dispatchers.IO + someCoroutineName + someExceptionHandler
```

 
관련 문맥

CoroutineName
- 코루틴에 이름을 주며 디버깅을 위해서 사용된다
```java
val someCoroutineName = CoroutineName("someCoroutineName")
```

Job
- 작업 객체를 지정할 수 있으며 취소가능 여부에 따라 SupervisorJob() 사용
```java
val parentJob = SupervisorJob() // or Job()
val someJob = Job(parentJob)
```


CoroutineDispatcher
- dispatchers.Default, ...IO, 등을 지정할 수 있으며 필요에 따라 스레드풀 생성 가능
```java
val MyPool = Executors.newFixedThredPool(2).asCoroutineDispatcher()
```

CoroutineExceptionHandler
- 코루틴 문맥을 위한 예외처리를 담당하며 코루틴에서 예외가 던져지면 처리한다.
- 예외가 발생한 코루틴은 상위 코루틴에 전달되어 처리될 수 있다.
    - 스코프를 가지는 경우 예외 에러를 잡아서 처리할 수 있다.
- 만일 예외처리가 자식에만 있고 부모에 없는 경우 부모에도 예외가 전달되므로 주의
    - 이 경우 앱이 깨지게(crash)됨
- 예외가 다중으로 발생하면 최초 하나만 처리하고 나머지는 무시됨

```java
// 예외 선언
val someExceptionHandler = CoroutineExceptionHandler { coroutineContext, throwable -> 
    val coroutineName = coroutineContext[CoroutineName]?.name ?: "default coroutine name"
    println("Error in $coroutineName: ${throwable.localizedMessage}")
}
```


### 코루틴의 스코프

GlobalScope
- 독립형(Standalone)코루틴을 구성, 생명주기는 프로그램 전체(top-level)에 해당하는 범위를 가지며 main의 생명 주기가 끝나면 같이 종료됨
- Dispatchers.Unconfined와 함께 작업이 서로 무관한 전역 범위 실행
- 보통 GlobalScope 상에서는 launch나 async 사용이 권장되지 않음

CoroutineScope
- 특정 목적의 디스패처를 지정한 범위를 블록으로 구성할 수 있다.
- 모든 코루틴 빌더는 CoroutineScope의 인스턴스를 갖는다.
- launch {...}와 같이 인자가 없는 경우에는 CoroutineScope에서 상위의 문맥이 상속되어 결정 (기본값)
- launch(Dispatchers.옵션인자) {...}와 같이 디스패처의 스케쥴러를 지정가능
    - Dispatchers.Default는 GlobalScope에서 실행되는 문맥과 동일하게 사용


코루틴 스코프 사용 예시

GlobalScope
```java
//GlobalScope
val scope1 = GlobalScope
// 권장하지 않는 사항
scope1.launch { ... } 
scope1.async { ... }
```
```java
//GlobalScope
GlobalScope.launch { ... }
val job1 = GlobalScope.launch { ... } // Job 객체 -> Job.join()으로 기다림
val job2 = GlobalScope.async { ... } // Deferred 객체 -> Deferred.await()으로 기다림(결과 반환)
```

CoroutineScope
```java
//CoroutineScope
val scope2 = CoroutineScope(Dispatcher.Default)
val routine1 = scope2.launch { ... }
val routine2 = scope2.async { ... }
```
```java
//CoroutineScope
launch(Dispatcher.Default) { ... } 
async(Dispatcher.Default) { ... } 
```


practice
```java
import kotlinx.coroutines.*

fun main() = runBlocking { // Main이 runBlocking 스코프가 된다. 일종의 하나의 코루틴 스코프 

    launch { // 단순히 launch의 인자를 사용하지 않음(생략함)으로써 상위의 문맥이 상속되게 한다.
        delay( 200L )
        println("Task from runBlocking)
    }

    coroutineScope {
        launch {
            delay( 200L )
            println("Task frm nested launch")
        }
        delay( 200L )
        println("Task from coroutineScope")
    }

    println("end of runBlocking")
}
```
```
Task from coroutineScope
Task from runBlocking
Task from nested launch
end of runBlocking
```

```java
import kotlinx.coroutines.*

fun main() = runBlocking { 

    launch { 
        delay( 1200L ) // 시간을 더 줘서 나중에 출력하도록 한다.
        println("Task from runBlocking)
    }

    coroutineScope {
        launch {
            delay( 200L )
            println("Task frm nested launch")
        }
        delay( 200L )
        println("Task from coroutineScope")
    }

    println("end of runBlocking")
}
```
```
Task from coroutineScope
Task from nested launch
end of runBlocking
Task from runBlocking 
```

```java
import kotlinx.coroutines.*

fun main() = runBlocking { 

    launch { 
        delay( 1200L ) 
        println("Task from runBlocking)
    }

    coroutineScope {
        launch {
            delay( 1500L ) /* 해당 시간을 늘려주면 coroutineScope의 시간 전체가 증가하므로
                            runBlocking의 launch가 실행될 여유가 생긴다.*/
            println("Task frm nested launch")
        }
        delay( 200L )
        println("Task from coroutineScope")
    }

    println("end of runBlocking")
}
```
```
Task from coroutineScope
Task from runBlocking
Task from nested launch
end of runBlocking
```

```java
import kotlinx.coroutines.*

fun main() = runBlocking(Dispatchers.Default) { // 필요에 따라 인자가 주어질 수 있다.

    launch(Dispatchers.IO) {  // launch 또한 필요에 따라 인자가 주어질 수 있다.
        delay( 1200L ) 
        println("Task from runBlocking)
    }

    coroutineScope { // 인자가 주어질 수 없다!
        launch {
            delay( 1500L )
            println("Task frm nested launch")
        }
        delay( 200L )
        println("Task from coroutineScope")
    }

    println("end of runBlocking")
}
```
```
Task from coroutineScope
Task from runBlocking
Task from nested launch
end of runBlocking
```
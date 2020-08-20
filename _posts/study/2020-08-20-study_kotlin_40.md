---
layout: post
author: study
title:  "Kotlin 객체편 - [30]"
description: "Kotlin 코루틴 - 빌더의 속성과 스코프의 부모-자식관계"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 스레드풀

스레드풀(thread pool)의 사용
- 보통 CommonPool이 지정되어 코루틴이 사용할 스레드의 공동 풀(pool)을 사용
- 이미 초기화되어 있는 스레드 중 하나 혹은 그 이상이 선택되며 초기화하기 때문에 스레드를 생성하는 오버헤드가 없어 빠름
- 하나의 스레드에 다수의 코루틴이 동작할 수 있다.

특정 스레드 개수를 직접 지정하는 경우
```java
val threadPool = Executors.newFixedThreadPool(4)
val MyContext = threadPool.asCoroutineDispatcher()
...
async(MyContext) { ... }
...
```


부모-자식 및 독립적인 스코프의 코루틴

예시
```java
fun main() = runBlocking<Unit> {
    println("runBlocking: $coroutineContext")
    val request = launch {
        println("requst: $coroutineContext")
        GlobalScope.launch { // 프로그램 전역으로 독립적인 수행 (부모-자식관계 없음)
            println("job1: before suspend function, $coroutineContext")
            delay(1000)
            println("job1: after suspend function, $coroutineContext") // 작업 취소에 영향을 받지 않음
        }

        launch { // 부모의 문맥을 상속(상위 launch의 자식)
            delay(100)
            println("job2: before suspend function, $coroutineContext")
            delay(1000)
            println("job2: after suspend function, $coroutineContext") // request(부모)가 취소되면 수행되지 않음
        }
    }
    delay(500)
    request.cancel() // 부모 코루틴의 취소
    delay(1000)
    println("end")

}
```
```
runBlocking: [Blocking...{Active}..., BlockingEventLoop...]
request: [Standalone...{Active}..., BlockingEventLoop...]
job1: before suspend function, [Standalone...{Active}..., DefaultDispatcher]
job2: before suspend function, [Standalone...{Active}..., BlockingEventLoop...]
job1: after suspend function, [Standalone...{Active}..., DefaultDispatcher]
end
```

```java
fun main() = runBlocking<Unit> {
    println("runBlocking: $coroutineContext")
    val request = launch {
        println("requst: $coroutineContext")
        GlobalScope.launch { 
            println("job1: before suspend function, $coroutineContext")
            delay(1000)
            println("job1: after suspend function, $coroutineContext") 
        }

        launch(Dispatchers.Default) { // 부모의 문맥을 상속(상위 launch의 자식), 분리된 작업
            delay(100)
            println("job2: before suspend function, $coroutineContext")
            delay(1000)
            println("job2: after suspend function, $coroutineContext") // 여전히 상속되었으므로 취소
        }
    }
    delay(500)
    request.cancel()
    delay(1000)
    println("end")

}
```
```
runBlocking: [Blocking...{Active}..., BlockingEventLoop...]
request: [Standalone...{Active}..., BlockingEventLoop...]
job1: before suspend function, [Standalone...{Active}..., DefaultDispatcher]
job2: before suspend function, [Standalone...{Active}..., DefaultDispatcher] // 달라진 부분
job1: after suspend function, [Standalone...{Active}..., DefaultDispatcher]
end
```

```java
fun main() = runBlocking<Unit> {
    println("runBlocking: $coroutineContext")
    val request = launch {
        println("requst: $coroutineContext")
        GlobalScope.launch { 
            println("job1: before suspend function, $coroutineContext")
            delay(1000)
            println("job1: after suspend function, $coroutineContext") 
        }

        CoroutineScope(Dispatchers.Default).launch { // 새로운 스코프가 구성되어 request와 무관
            delay(100)
            println("job2: before suspend function, $coroutineContext")
            delay(1000)
            println("job2: after suspend function, $coroutineContext") // 무관하므로 실행된다.
        }
    }
    delay(500)
    request.cancel()
    delay(1000)
    println("end")

}
```
```
runBlocking: [Blocking...{Active}..., BlockingEventLoop...]
request: [Standalone...{Active}..., BlockingEventLoop...]
job1: before suspend function, [Standalone...{Active}..., DefaultDispatcher]
job2: before suspend function, [Standalone...{Active}..., DefaultDispatcher]
job1: after suspend function, [Standalone...{Active}..., DefaultDispatcher]
job2: after suspend function, [Standalone...{Active}..., DefaultDispatcher]
end
```



실행방법의 비교
1.
```java
fun work(i: Int) {
    Thread.sleep(1000)
    println("Work $i done")
}

fun main() {
    val time = measureTimeMillis {
        runBlocking {
            for (i in 1..2) {
                launch {
                    work(i)
                }
            }
        }
    }
    println("Done in $time ms")
}
```
```
Work 1 done
Work 2 done
Done in 2268 ms
```
생성된 두개의 작업은 단일 스레드에서 순차적으로 실행 된다.

2.
```java
fun work(i: Int) {
    Thread.sleep(1000)
    println("Work $i done")
}

fun main() {
    val time = measureTimeMillis {
        runBlocking {
            for (i in 1..2) {
                launch(Despatchers.Default) {
                    work(i)
                }
            }
        }
    }
    println("Done in $time ms")
}
```
```
Work 1 done
Work 2 done
Done in 1260 ms
```
생성된 두개의 작업은 동시성을 제공하여 분리된 작업으로 실행된다.

3.
```java
fun work(i: Int) {
    Thread.sleep(1000)
    println("Work $i done")
}

fun main() {
    val time = measureTimeMillis {
        runBlocking {
            for (i in 1..2) {
                GlobalScope.launch {
                    work(i)
                }
            }
        }
    }
    println("Done in $time ms")
}
```
```
Done in 118 ms
```
부모와 무관한 독립 실행이므로 작업 완료를 기다리지 않는다. (join()으로 기다려줘야 한다.)


### 빌더의 특정 속성 지정

시작 시점에 대한 속성 - launch의 원형
```java
public fun launch(
    context: CoroutineContext,
    start: CoroutineStart,
    parent: Job?,
    onCompletion: CompletionHandler?,
    block: suspend CoroutineScope.() -> Unit): Job {
        ...
    }
)
```

CooroutineStart
- DEFAULT: 즉시 시작(해당 문맥에 따라 즉시 스케쥴링 됨)
- LAZY: 코루틴을 느리게 시작(처음에는 중단된 상태이며 start()나 await() 등으로 시작)
- ATOMIC: 원자적으로 즉시 시작(DEFAULT와 비슷하나 코루틴을 실행저에는 취소 불가)
- UNDISPATCHED: 현재 스레드에서 즉시 시작(첫 지연함수까지, 이후 재개시 디스패치 된다)


start() 혹은 await()가 호출될 때 실제로 시작
- launch나 async는 즉시 실행되지만 start 옵션에 따라 실행시점을 늦출 수 있다.
```java
val job = async(start = CoroutineStart.LAZY) { doWork1() }
...
job.start() // 실제 시작 시점으로 또는 job.await()으로 시작됨
```

- 안드로이드의 버튼을 통한 시작 예
```java
val someJob = launch(start = CoroutineStart.LAZY) {
    // some code //
}
someButton.setOnClickListener {
    someJob.start()
}
```
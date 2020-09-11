---
layout: post
author: study
title:  "Kotlin 객체편 - [31]"
description: "코루틴 - 블로킹과 문맥 변환"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 완료를 기다리기 위한 블로킹

runBlocking의 사용
- 새로운 코루틴을 실행하고 완료되기 전까지는 현재(caller) 스레드를 블로킹
- 코루틴 빌더와 마찬가지로 CoroutineScope의 인스턴스를 가짐
```java
fun <T> runBlocking(
    context: CoroutineContext = EmptyCoroutineContext,
    block: suspend CoroutineScope.() -> T
): T (source)
```


main()을 블로킹 모드로 동작시키기
```java
import kotlinx.coroutines.*

fun main() = runBlocking<Unit> { // 메인 메서드가 코루틴 환경에서 실행
    launch { // 백그라운드로 코루틴 실행
        delay( 1000L )
        println("World!")
    }
    println("Hello") // 즉시 이어서 실행됨
    // delay( 2000L ) // delay()를 사용하지 않아도 코루틴을 기다림

}
```

runBlocking()을 클래스 내의 멤버 메서드에서 사용할 때
```java
class MyTest {
    ...
    fun mySuspendMethod() = runBlocking<Unit> {
        // 코드
    }
}
```

특정 디스패처 옵션을 주어줄 때
```java
runBlocking(Dispatchers.IO) {
    launch {
        repeat(5) {
            println("counting ${it + 1}")
            delay(1000)
        }
    }
}
```

### 특정 문맥과 함께 실행
withContext()
- 인자로 코루틴 문맥을 지정하며 해당 문맥에 따라 코드 블록을 실행
- 해당 코드 블록은 다른 스레드에서 수행되며 결과를 반환 한다.
- 부모 스레드는 블록하지 않는다.

```java
// 함수 원형
suspend fun <T> withContext(
    context: CoroutineContext,
    block: suspend CoroutineScope.() -> T
): T (source)
```

예 
```java
resultTwo = withContext(Dispatchers.IO) { function2() }
```


완료보장
- withContext(NonCancellable) {...}
    - try { ... } finally { ... } 에서 finally 블록의 실행을 보장하기 위해 취소불가 블록 구성



### 스코프 빌더
coroutineScope 빌더
- 자신만의 코루틴 스코프(scope)를 선언하고 생성할 수 있다.(구조화된 코루틴)
- 모든 자식이 완료되기 전까지는 생성된 코루틴 스코프는 종료되지 않는다.
- runBlocking과 유사하지만 runBlocking은 단순 함수로 현재 스레드를 블록킹, coroutineScope는 단순히 지연(suspend)함수 형태로 넌블로킹으로 사용됨

```java
suspend fun <R> coroutineScope(
    block: suspend CoroutineScope.() -> R
): R (source)
```

- 만일 자식 코루틴이 실패하면 이 스코프도 실패하고 남은 모든 자식은 취소(cancel)된다. (반면에 supervisorScope는 실패하지 않음)
    - 외부에 의해 작업이 취소되는 경우 CancellationException 발생


supervisorScope 빌더
- 마찬가지로 코루틴 스코프를 생성하며 이때 SupervisorJob과 함께 생성하여 기존 문맥의 Job을 오버라이드 한다.
    - launch를 사용해 생성한 작업의 실패는 CoroutineExceptionHandler를 통해 핸들링
    - async를 사용해 생성한 작업의 실패는 Deferred.await의 결과에 따라 핸들링
    - parent를 통해 부모작업이 지정되면 자식작업이 되며 이때 부모에 따라 취소여부 결정
```java
fun SupervisortJob(parent: Job? = null): CompletableJob ( source )
```
-  자식이 실패하더라도 이 스코프는 영향을 받지 않으므로 실패하지 않는다.
    - 실패를 핸들링하는 정책을 구현할 수 있다.
- 예외나 의도적인 취소에 의해 이 스코프의 자식들을 취소하지만 부모의 작업은 취소하지 않는다.


예시
```java
import kotlinx.coroutines.*

fun main() = runBlocking { // this: CoroutineScope
    launch {
        delay( 200L )
        println("Task from runBlocking)
    }

    coroutineScole { // 코루틴 스코프의 생성 -- 부모가 하위 코루틴 완료를 기다림
    // CoroutineScope(Dispatchers.Default).launch { // -- 이때는 부모-자식 관계와 무관(완료를 기다리지 않음)
        launch {
            delay( 500L )
            println("Task from nested launch")
        }

        delay( 100L )
        println("Task from coroutine scope")
    }

    println("Coroutine scope is over")
}
```


### 부모와 자식 코루틴 관의 관계

병렬 분해(Parallel decomposition)
```java
suspend fun loadAndCombine(name1: String, name2: String): Image {
    val deferred1 = async { loadImage(name1) }
    val deferred2 = async { loadImage(name2) }
    return combineImages(deferred1.await(), deferred2.await())
}
```
- loadImage(name2)는 여전히 진행된다.
- 코루틴 문맥에서 실행하여 자식 코루틴으로 구성한다면 예외를 부모에 전달하고 모든 자식 코루틴을 취소할 수 있다.
```java
suspend fun loadAndCombine(name1: String, name2: String): Image = 
coroutineScope { // 스코프를 설정함으로 특정 자식 코루틴의 취소가 생기면 모든 자식을 취소하게됨
    val deferred1 = async { loadImage(name1) }
    val deferred2 = async { loadImage(name2) }
    combineImages(deferred1.await(), deferred2.await())
}
```

### 스코프의 취소와 예외처리
```java
val scope2 = CoroutineScope
val routine1 = scope2.launch { ... }
val routine2 = scope2.async { ... }

scope2.cancel()
/* 또는
val scope2 = CoroutineScope
val routine1 = scope2.launch { ... }
val routine2 = scope2.async { ... }

scope2.cancelChildern()
*/
```
```
try {
    ...
} catch ( e: CancellationExceoption) {
    ... 취소에 대한 예외처리 ...
}
```

### 코루틴의 실행 시간 지정
실행 시간 제한
- withTimeout(시간값) { ... } = 특정 시간값 동안만 수행하고 블록을 끝냄
    - 시간값이 되면 TimeoutCancellationException 예외를 발생
- withTimeoutOrNull(시간값) { ... } - 동작은 위와 동일
    - 단, 예외를 발생하지 않고 null을 반환


```java
val result = withTimeoutOrNull( 1300L ) {
    repeat(1000) { i =>
        println("I'm sleeping $i ...")
        delay(500L)
    }
    "Done" // 코루틴 블록이 취소되면 이 값이 result에 반환됨
}
println("Result is $result")
```


### 요약

- MainScope
MainScope는 UI 컴포넌트를 위한 스코프입니다. MainScope는 SupervisorJob을 생성하고 이 스코프에서 만들어진 모든 코루틴을 Main스레드에서 실행합니다. SupervisorJob을 생성하기 때문에 하나의 코루틴이 예외를 던지면서 실패해도 다른 코루틴들은 취소되지 않습니다.

자식 코루틴의 실패는 CoroutineExceptionHandler를 사용하여 처리 할 수 있습니다.

- CoroutineScope(context)
CoroutineScope(context) 함수는 제공된 코루틴 문맥에서 새로운 코루틴 스코프를 생성합니다. 
 

- 블로킹 모드의 실행 
runBlocking은 새로운 코루틴을 시작하고 코루틴이 완료될 때까지 현재 스레드를 차단합니다. 일시 중단 및 논블록킹 스타일로 작성된 일반 블록 코드와 라이브러리를 연결하도록 설계되었습니다. 



코루틴의 문맥들

- 기본 문맥
Dispatchers.Default는 기본 문맥인 CommonPool에서 실행되기 때문에 새로운 스레드를 생성하지 않고 기존에 있는 것을 이용합니다. 그러므로 연산 중심의 코드에 적합합니다.

- I/O를 위한 문맥
Dispatchers.IO는 입출력에 적합한 공유 풀로써, 볼로킹 동작이 많은 파일 or 소켓 I/O 처리에 사용하면 좋습니다.

- Unconfined 문맥
비한정 문맥에서 실행된 코루틴은 첫번째 중단점을 만날때까지만 호출자 스레드에서 실행됩니다. 중단점 이후에 재개 되었을때는 서스펜드 함수가 실행된 스레드에서 수행됩니다. 그렇기 때문에 비한정 문맥은 예측 불능한 상태로 수행되기 때문에 해당 기능을 사용하는 것은 권장되지 않습니다.

- 새 스레드를 생성하는 문맥
newSingleThreadContext는 새로운 스레드가 생성되기 때문에 비용이 많이 들고 더 이상 필요하지 않으면 해제하거나 종료시켜야 합니다. 부모 코루틴이 취소되는 경우 자식 코루틴도 재귀적으로 취소됩니다.


따라서 필요한 경우 join()함수를 사용하여 명시적으로 처리를 기다리도록 만들 수 있습니다.

 

문맥 바꾸기 - withContext(context)
- withContext(context) : 현재 코 루틴의 컨텍스트를 전환합니다. 주어진 블록이 실행될 때 코 루틴은 이전 컨텍스트로 다시 전환됩니다.
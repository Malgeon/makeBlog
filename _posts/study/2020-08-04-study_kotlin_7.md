---
layout: post
author: study
title:  "Kotlin 함수편 - [7]"
description: "반복문"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### for문
자바와 같은 세미콜론을 이용한 for문은 사용할 수 없다.

```javascript
for (x in 1..5) { // 1->5
    println(x)
}

for (x in 1..5) println(x) // if문과 동일하게 한줄 표현도 가능
```

하행 반복

```javascript
// for (x in 5..1) println(x) // 잘못된 사용! 아무것도 출력되지 않는다.
for (x in 5 downTo 1) println(x)
```

필요한 단계 증가

```javascript
for (x in 1..5 step 2) println(x) // 1 3 5
for (x in 5 downTo 1 step 2) println(x) // 5 3 1
```

until 

```javascript
for (x in 1 until 6) println(x) // 6 전까지만 간다.
```

### 흐름 제어

#### return
- return으로 Unit 이 가능하다.(기억해두자.)

람다식에서 return 사용하기

우선 inline으로 선언되지 않는 람다식 함수에서는 그냥 return은 사용이 가능하지 않다. 람다식 특성상 마지막 식이 반환되기 때문이다. 그렇지만 흐름제어를 하다보면 return이 필요한 순간이 찾아온다. inline으로 람다식을 구성하면 return이 가능하지만, 비지역 반환이 일어나게 되어 람다식 함수가 끝남과 동시에 람다식 함수를 가지고 있는 함수까지도 return이 이루어지게된다. 

```javascript
fun main() {
    retFunc()
}

inline fun inlineLambda(a: Int, b: Int, out: (Int, Int) -> Unit) {
    out(a, b)
}

fun retFunc() {
    println("start of retFunc")
    inlineLambda(13, 3) { a, b -> 
        val result = a + b
        if(result > 10) return // retFunc자체가 return
        println("result: $result")
    }
    println("end of retFunc") // 10 을 초과하면 해당 println도 실행되지 않는다. 
}
```

람다식 함수 내에서만 return이 이루어 질 수 있도록 해보자.

람다식에서 라벨 사용(물론 inline함수에서도 사용 가능하다)

람다식 함수명 라벨이름@ {
    ...
    return@라벨이름
}

```javascript
fun main() {
    retFunc()
}

fun lambda(a: Int, b: Int, out: (Int, Int) -> Unit) {
    out(a, b)
}

fun retFunc() {
    println("start of retFunc")
    lambda(13, 3) lit@{ a, b -> 
        val result = a + b
        if(result > 10) return@lit //람다식 내부에서 return이 실행됨.(지역 반환)
        println("result: $result")
    }
    println("end of retFunc") // result가 10이 넘더라도 해당 구문 실행됨.
}
```

암묵적 라벨
라벨에 이름을 붙여주는 대신 함수이름을 사용할 수 있다.

```javascript
fun main() {
    retFunc()
}

fun lambda(a: Int, b: Int, out: (Int, Int) -> Unit) {
    out(a, b)
}

fun retFunc() {
    println("start of retFunc")
    lambda(13, 3) { a, b -> 
        val result = a + b
        if(result > 10) return@lambda //람다식 내부에서 return이 실행됨.(지역 반환)
        println("result: $result")
    }
    println("end of retFunc") // result가 10이 넘더라도 해당 구문 실행됨.
}
```

사실 익명 함수를 사용해서 해결할 수 있다.

```javascript
fun retFunc() {
    println("start of retFunc")
    lambda(13, 3, fun (a, b) { 
        val result = a + b
        if(result > 10) return
        println("result: $result")
    })
    println("end of retFunc") // result가 10이 넘더라도 해당 구문 실행됨.
}
```
익명 함수를 사용하게 되면 코딩의 직관성이 떨어져서, 주의해서 사용하자.


람다식과 익명 함수 비교

```javascript
val getmessage = lambda@ { num: Int ->
    if(num !in 1..100) {
        return@lambda "Error"
    }
    "Sucess"// 마지막 식이 반환
}
```
```javascript
val getmessage = fun(num: Int): String {
    if(num !in 1..100) {
        return "Error"
    }
    return "Sucess"
}

val result = getMessage(99)
```


### break & continue with label

```javascript
fun labelBreak() {
    first@ println("labelBreak")
    for(i in 1..5) {
        second@ for (j in 1..5) {
            if (j == 3) break // @second에서 break.
            println("i: $i, j:$j")
        }
        println("after for j")
    }
    println("after for i")
}
```

```javascript
fun labelBreak() {
    first@ println("labelBreak")
    for(i in 1..5) {
        second@ for (j in 1..5) {
            if (j == 3) break@first // @first에서 break.!!
            println("i: $i, j:$j")
        }
        println("after for j")
    }
    println("after for i")
}
```

continue도 동일하다!
---
layout: post
author: study
title:  "Kotlin 함수편 - [8]"
description: "예외처리"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
 try {
     예외 발생 가능성 있는 문장
 } catch (e: 예외처리 클래스명) {
     예외를 처리하기 위한 문장
 } finally {
     반드시 실행되어야 하는 문장
 }

 try에서 예외로 인해 반드시 실행되어야 하는 문장이 필요할 경우 finally를 이용한다. 없다면 생략 가능.

 ### 특정 예외 처리
 산술 연산에 대한 예외를 따로 특정해서 잡고자 한다면

```javascript
 ...
 } catch (e: ArithmeticException) {
     println("Exception is handled. ${e.message}" ) // 0으로 나눌경우 
     //Exception is handled. / by zero 를 리턴.
 }
```

스택의 추적

```javascript
 ...
 } catch (e: Exception) {
     e.printStackTrace() // 0으로 나눌 경우
    /**
    java.lang.ArithmeticException : / bt zero
        at ...(패키지 및 주소).main
    */
}
```

특정 조건에 따른 예외 발생

```
throw Exception(message: String)
```

```javascript
fun main() {
    var amount = 600

    try {
        amount -= 100
        checkAmount(amount)
    } catch (e : Exception) {
        println(e.message)
    }
    println("amount: $amount")
}

fun checkAmount(amount: Int) {
    if (amount < 1000)
        throw Exception("잔고가 $amount 으로 1000 이하입니다.")
}
```



 
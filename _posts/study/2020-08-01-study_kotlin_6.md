---
layout: post
author: study
title:  "Kotlin 함수편 - [6]"
description: "Kotlin 조건문"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### if~else 문의 간략화

```javascript
var max: Int
if (a > b) 
    max = a
else 
    max = b
```

```javascript
var max = if (a > b) a else b
```

블록과 함께 사용할 경우

```javascript
fun main() {
    val a = 12
    val b = 7

    val max = if (a > b) {
        println("a 선택")
        a // 블럭의 마지막 식이 반환되어 max에 할당.
    }
    else {
        println("b 선택")
        b // 블럭의 마지막 식이 반환되어 max에 할당.
    }

    println(max)
```

## readLine()콘솔로부터 입력 받는 함수
```javascript
val score = readLine()!!.toDouble() // readLine은 Null이 될수도 있으므로 Null값 처리를 해줘야 한다. 여기선 잠깐 테스트를 위한 것이라 !!로 처리.
```

## 범위 연산자
변수명 in 시작값..마지막값
```javascript
if(score in 80..89) // score가 80~89 사이에 있는 값인지 확인(마지막 값은 포함!)
```

## when문(switch ... case과 비슷)
범위 연산자, is 키워드 사용 가능.
```javascript
when(x) {
    1 -> print("x == 1") //break 필요없이 빠져나온다.
    2 -> print("x == 2")
    3, 4 -> print("x == 3, 4") //일치되는 여러 조건 사용 가능
    parseInt(s) -> print("일치함!") // 함수의 반환값 사용 가능
    in 10..20 -> print("x는 10이상 20이하입니다.") //범위 연산자 사용 가능
    !in 10..20 -> print("x는 10이상 20이하의 범위에 포함되지 않습니다.")
    else -> { // 블록 구문 사용 가능.
        print("else")
    }
}

val str = "안녕하세요."
val result = when(str) {
    is String -> "문자열입니다."
    else -> false
}
```

인자가 없는 when

```javascript
when {
    score <= 90.0 -> grade = 'A'
    score in 80.0..89.9 -> grade = 'B'
    score in 70.0..79.9 -> grade = 'C'
    score < 70.0 -> grade = 'F'
}
```
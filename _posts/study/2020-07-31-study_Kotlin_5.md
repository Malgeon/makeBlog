---
layout: post
author: study
title:  "Kotlin 함수편 - [5]"
description: "익명함수, 인라인함수, 확장함수, 중위표현법, 꼬리재귀함수"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### 익명 함수 

```javascript
fun (x: Int, y; Int): Int = x + y // 함수의 이름이 생략되어 있다.
```
```javascript
val add: (Int, Int) -> Int = fun(x, y) = x + y
val result = add(10, 2) // 임시적으로 function이 필요할 경우 저런식으로 사용한다.
```
```javascript
val add = fun(x: int, y: Int) = x + y // 아래의 람다식과 매우 흡사하다. 그러나 return, break, continue는 람다식에서 사용불가능 하지만 일반 익명 함수에서는 사용이 가능하다.
```
```javascript
val add = {x: Int, y: Int -> x + y } 
```


### 인라인 함수
함수가 호출되는 곳에 내용을 모두 복사
함수의 분기 없이 처리 -> 성능 증가

그러나 인라인 되는 함수의 코드가 많아질수록 복사된 전체 코드량이 증가하게 되므로 오히려 성능이 줄어들게 된다. 성능 향상 측면에서 람다식 함수에 사용될 것을 권장하고 있다.

```javascript
inline fun shortFunc(a: Int, out: (Int) -> Unit) {
    println("Hello")
    out(a)
}
fun main() {
    shortFunc(3) {
        println("a: $it")
    }
}
```

noinline
일부 람다식 함수를 인라인 되지 않게 하기 위함.

```javascript
inline fun sub(out1: () -> Unit, noinline out2 () -> Unit) {
    ...
}
```

crossinline

```javascript
inline fun shortFunc(a: Int, out: (Int) -> Unit) {
    println("Hello")
    out(a)
    println("Goodbye")
}
fun main() {
    shortFunc(3) {
        println("a: $it")
        return // return에 의해 Goodbye가 실행되지 않는다.
    }
}
```

crossinline을 사용하므로써 비지역반환을 금지하게 한다.

```javascript
inline fun shortFunc(a: Int, crossinline out: (Int) -> Unit) {
    println("Hello")
    out(a)
    println("Goodbye")
}
fun main() {
    shortFunc(3) {
        println("a: $it")
        //return 사용시 error
    }
}
```

### 확장 함수(extension function)
클래스의 멤버 함수를 외부에서 더 추가할 수 있다.
적절히 사용하면 유용하지만 남발하면 호환성이 떨어지게 된다.

fun 확장대상.함수명(매개변수, ...): 반환값 {
    ...
    return 값
}

```javascript
fun main() {
    val source = "Hello World!"
    var target = "Kotlin"
    println(source.getLongString(target))
}

fun String.getLongString(target: String): String = 
    if (this.length > target.length) this else target
```

### 중위 표현법(infix notation)
클래스의 멤버 호출 시 사용하는 점(.)을 생략하고 함수 이름 뒤에 소괄호를 생략해 직관적인 이름을 사용할 수 있는 표현법

조건
- 멤버 메서드 또는 확장 함수여야 한다.
- 하나의 매개변수를 가져야 한다.
- infix 키워드를 사용하여 정의한다.

```javascript
fun main() {
    // 일반 표현법
    // val multi = 3.multiply(10)
    
    // 중위 표현법
    val multi = 3 multiply 10
    println("multi: $multi")
}

infix fun Int.multiply(x: Int): Int { 
    return this * x
}
```

### 꼬리 재귀 함수(tail recursive function)
스택에 계속 쌓이는 방식이 함수가 계속 씌워지는 꼬리를 무는 형태
코틀린 고유의 tailrec 키워드를 사용해 선언

```javascript
//일반 재귀
fun main() {
    val number = 4
    val result: Long

    result = factorial(number)
    println("Factorial: $number -> $result")
}

fun factorial(n: Int): Long {
    return if(n == 1) n.toLong() else n * factorial(n-1)
}
```

```javascript
//꼬리 재귀
fun main() {
    val number = 5   
    println("Factorial: $number -> ${factorial(number)}")
}

tailrec fun factorial(n: Int, run: Int = 1): Long {
    return if(n == 1) run.toLong() else factorial(n-1, run*n) //값을 꼬리물며 들어가기 때문에 스택을 사용하지 않는다.
}
```
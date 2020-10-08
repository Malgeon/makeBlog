---
layout: post
author: study
title:  "Kotlin 함수편 - [9]"
description: "표준함수 - closure, let, also, apply"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 

### closure
- 람다식으로 표현된 내부 함수에서 외부 범위에 선언된 변수에 접근할 수 있는 개념
- 람다식 안에 있는 외부 변수는 값을 유지하기 위해 람다가 포획(capture)한 변수

```javascript
fun main() {
    val calc = Calc()
    var result = 0 // 람다식 외부 변수

    calc.addNum(2, 3) { x, y -> result = x + y} // closure
    println(result) // 값을 유지하여 5가 출력된다!
}

class Calc {
    fun addNum(a: Int, b: Int, add: (Int, Int) -> Unit) {
        add(a, b)
    }
}
```

```javascript
// 길이가 일치하는 이름만 반환
fun filteredNames(length: Int) {
    val names = arrayListOf("Kim", "Hong", "Go", "Hwang", "Jeon")
    val filterResult = names.filter {
        it.length == length // 바깥의 length에 접근
    }
    println(filterResult)
}
```

### let()
- 함수를 호출하는 객체 T를 이어지는 block의 인자로 넘기고 block의 결과값 R을 반환

`public inline fun <T, R> T.let(block: (T) -> R):R { ... return block(this)}`

- 매개변수 block은 T를 매개변수로 받아 R을 반환
- let()함수 역시 R을 반환
- 본문의 this는 객체 T를 가리키는데 람다식 결과 부분을 그대로 반환한다는 뜻
- 다른 메서드를 실행하거나 연산을 수행해야 하는 경우 사용

```javascript
fun main() {
    val score: Int? = 32
    //var score = null

    // 일반적인 null 검사
    fun checkScore() {
        if (score != null) {
            println("Score: $score")
        }
    }

    // let을 사용해 null 검사를 제거
    fun checkScoreLet() {
        score?.let { println{"Score: $it"} }
        val str = score.let { it.toString() }
        println(str)
    }
    checkScore()
    checkScoreLet()
}
```
let 함수의 체이닝(chaining)

```javascript
fun main() {
    var a = 1
    val b = 2

    a = a.let { it + 2 }.let {
        println("a = $a")
        val i = it + b
        i
    }

    println(a) // 5
}
```

let의 중첩 사용
```javascript
var x = "Kotlin!"
    x.let { outer ->
        outer.let { inner ->
            print("Inner is $inner and outer is $outer") // 이떄는 it을 사용하지 않고 명시적 이름을 사용한다.
        }
    }
```

반환값은 바깥쪽의 람다식에만 적용
```javascript
var x = "Kotlin!"
    x.let { outer ->
        outer.let { inner ->
            print("Inner is $inner and outer is $outer")
            "Inner String" // 이건 반환 되지 않는다.
        }
        "Outer String" // 이 문자열이 반환되어 x에 할당된다.
    }
```

let의 활용 - null 검사
```javascript
var obj: String?
...
if (null != obj) { // 기존 방식
    Toast.makeText(applicationContext, obj, Toast.LENGTH_LONG).show()
}
```
```javascript
var obj?.let { // obj가 null 아닐경우 작업 수행 (Safe calls + let)
    Toast.makeText(applicationContext, it, Toast.LENGTH_LONG).show()
}
```

```javascript
val firstName: String?
var lastName: String
...
if (null != firstName) { // 기존 방식
    print("$firstName $lastName")
} else {
    print("$lastName")
}
```

```javascript
var firstName?.let { // obj가 null 아닐경우 작업 수행 (Safe calls + let)
    print("$it $lastName") } ?: print("$lastName")
```

### also()

`public inline fun <T> T.also(block: (T) -> Unit): T { block(this); return this }` 

함수를 호출하는 객체 T를 이어지는 block에 전달하고 객체 T 자체를 반환.(let은 블록의 결과값 반환)

```javascript
var m = 1
m = m.also { it + 3 }
println(m) // 1
```

let 과 also 비교

```javascript
fun main() {
    data class Person(var name: String, var skills: String)
    var person = Person("Kildong", "Kotlin")

    val a = person.let {
        it.skills = "Anroid"
        "success"
    }

    println(person)
    println("a: $a") // success

    val b = person.also {
        it.skills = "Java"
    }

    println(person)
    println("b: $b") // person이 실행
}
```

also 활용 
```javascript
// 기존 함수
fun makeDIr(path: String): FIle {
    val result = File(path)
    result.mkdirs()
    return result
}
```
```javascript
// let과 also 활용
fun makeDIr(path: String) = path.let { File(it) }.also{ it.mkdirs() }
```
왜 let을 쓰고 also를 쓰는지 잘 생각해 보자.


### apply()

`public inline fun <T> T.apply(block: T.() -> Unit): T { block(); return this }`

also() 함수와 동일하게 호출하는 객체 T를 이어지는 block으로 전달하고 객체 자체인 this를 반환.
also는 block에서 람다식으로 처리하지만 apply는 확장 함수로 처리한다. (it을 사용하지 않고, this를 사용)

```javascript
fun main() {
    data class Person(var name: String, var skills: String)
    var person = Person("Kildong", "Kotlin")

    person.apply { this.skills = "Swift" }
    println(person)

    val returnObj = person.apply {
        name = "Sean" // this는 생략할수 있음
        skills = "Java" // this 없이 객체의 멤버에 여러번 접근 가능
    }

    println(person)
    println(returnObj) // person이 실행
}
```

apply 활용

```javascript
// 기존 코드
val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT)
param.gravity = Gravity.CENTER_HORIZONTAL
param.weight = 1f
param.topMargin = 100
param.bottomMargin = 100
```

```javascript
val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT).apply {
    gravity = Gravity.CENTER_HORIZONTAL
    weight = 1f
    topMargin = 100
    bottomMargin = 100
}
```

디렉토리 생성

```javascript
// 기존 함수
fun makeDIr(path: String): FIle {
    val result = File(path)
    result.mkdirs()
    return result
}
```
```javascript
File(path).apply { mkdirs() }
```


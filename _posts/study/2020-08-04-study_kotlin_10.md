---
layout: post
author: study
title:  "Kotlin 함수편 - [10]"
description: "Kotlin 표준함수활용 - 2"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### run()
run() 함수는 두 가지 형태이다.
`public inline fun <R> run(block: () -> R):R = return block()`
``public inline fun <T, R> T.run(block: T.() -> R):R = return block()`

```javascript
var skills = "Kotlin"
println(skills) // Kotlin

val a = 10
skills = run {
    val level = "Kotlin Level:" + a
    level //마지막 표현식이 반환됨
}
println(skills) // Kotlin Level: 10
```

apply 와 run 비교
```javascript
fun main() {
    data class Person(var name: String, var skills: String)
    var person = Person("Kildong", "Kotlin")

    val returnObj = person.apply {
        this.name = "Sean"
        skills = "java"
        "success" //사용되지 않는다.
    }
    println(person)
    println("returnObj: $returnObj") // person

    val returnObj2 = person.run {
        this,name = "Dooly"
        this.skills = "C#"
        "success"
    }
    
    println(person)
    println("returnObj2: $returnObj2") // success
}
```


### with()
run()함수와 기능이 거의 동일한데, with()에서는 receiver로 전달할 객체를 처리한다.

`public inline fun <T, R> with(receiver: T, block: T.() -> R): R = receiver.block()`

with는 세이프 콜(?.)을 지원하지 않기 떄문에 let과 같이 사용된다.
```javascript
supportActionBar?.let {
    with(it) {
        setDisplayHomeAsUpEnabled(true)
        setHomeAsUpIndicator(R.drawable.ic_clear_white)
    }
}
```
아래와 동일하다.
```javascript
supportActionBar?.run {
    setDisplayHomeAsUpEnabled(true)
    setHomeAsUpIndicator(R.drawable.ic_clear_white)
}
```


```javascript
fun main() {
    data class User(var name: String, var skills: String, var email: String? = null)

    var user = User("Kildong", "default")

    val result = with (user) {
        this.skills = "Kotlin"
        email = "kildong@example.com"
    }
    println(user) // User
    println("result: $result")  // result: kotlin.Unit (Unit을 반환한다.)
}
```

### use()
use()를 사용하면 객체를 사용한 후 close() 등을 자동적으로 호출해 닫아준다.

`public inline fun <T : Closeable?, R> T.use(block: (T) -> R) : R` 또는
`public inline fun <T : AutoCloseable?, R> T.use(block: (T) -> R) : R`

- T의 제한된 자료형을 보면 Closeable?로 block은 닫힐 수 있는 객체를 지정해야 함
- Java 7 이후는 AutoCloseable?로 사용됨

```java
//기존 자바 코드
private String readFirstLine() throws FileNotFoundException {
    BufferedReader reader = new BufferedReader(new FileReader("test.file"));
    try {
        return reader.readLine();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    return null;
}
```

```javascript
private fun readFirstLine(): String {
    BufferedReader(FileReader("test.file")).use { return it.readLine() }
}
```

파일 닫기에 대한 처리
```javascript
fun main() {
    PrintWriter(FileOutputStream(".....\output.txt")).use {
        it.println("hello")
    }
}
```
---
layout: post
author: study
title:  "Kotlin 객체편 - [4]"
description: "Getter, Setter"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 

### Getter & Setter

코틀린에서 게터와 세터가 작동하는 방식

- 접근 매서드는 생략(내부적으로 생성된다)

```javascript
class User(_id: Int, _name: String, _age: Int) {
    // 프로퍼티들
    val id: Int = _id 
    var name: String = _name
    var age: Int = _age
}
```

좀더 간략히

```javascript
class User(val id: Int, var name: String, var age: Int)
```


```javascript
fun main() {
    val user = User(1, "Sean", 30)

    // 게터에 의한 값 획득
    val name = user.name
    
    // 세터에 의한 값 지정
    user.age = 41

    println("name: $name, ${user.age}")
}
```

### Custom: Getter & Setter

게터와 세터가 포함되는 프로퍼티 선언 구조

```
var 프로퍼티이름[: 프로퍼티자료형] [= 프로퍼티 초기화]
    [get() { 게터 본문 } ]
    [set(value) { 세터 본문 } ]

val 프로퍼티이름[: 프로퍼티자료형] [= 프로퍼티 초기화]
    [get() { 게터 본문 } ]
```

불변형인 val은 게터만 설정 가능하다.

```javascript
class User(_id: Int, _name: String, _age: Int) {
    // 프로퍼티들
    val id: Int = _id 
        get() = field // 이곳에 id 를 입력해 버리면 코틀린 내부적으로 getter가 동작하여 무한 재귀 호출에 빠지게 된다.

    var name: String = _name
        get() = field
        set(value) {
            field = value
        }

    var age: Int = _age
        get() = field
        set(value) {
            field = value
        }
}

fun main() {
    val user = User(1, "Kildong", 30)
    
    // user.id = 2 // 에러! val 프로퍼티는 값 변경 불가
    user.age = 41
    println("name: $name, ${user.age}") // 게터 동작
}
```

value: 세터의 매개변수로 외부로 부터 값을 가져온다.
- 외부의 값을 받을 변수가 되므로 value 대신 어떤 이름이든지 상관 없다.

feild: 프로퍼티를 참조하는 변수로 보조 필드(backing field)로 불린다.
- 프로퍼티를 대신할 임시 필드로 만일 프로퍼티를 직접 사용하면 게터나 세터가 무한 호출되는 재귀에 빠지게 된다.

사실 위의 코드는 getter와 setter의 동작 코드이기 때문에 중복이라고 할수 있다.

조금 더 변경해보자


```javascript
class User(_id: Int, _name: String, _age: Int) {
    // 프로퍼티들
    val id: Int = _id 
        get() = field // 이곳에 id 를 입력해 버리면 코틀린 내부적으로 getter가 동작하여 무한 재귀 호출에 빠지게 된다.

    var name: String = _name
        get() = field
        set(value) {
            println("The name was changed")
            field = value.toUpperCase()
        }

    var age: Int = _age
        get() = field
        set(value) {
            field = value + 10
        }
}

fun main() {
    val user = User(1, "Kildong", 30)
    user.name = "coco" // 여기서 사용자 고유의 출력 코드가 실행
    println("name: $name, ${user.age}") // 게터 동작
}
```

임시적인 보조 프로퍼티

```javascript
class User(_id: Int, _name: String, _age: Int) {
    // 프로퍼티들
    val id: Int = _id 
    private var tempName: String? = null

    var name: String = _name
        get() {
            if (tempname == null) tempName = "NONAME"
            return tempName ?: throw AsertionError("Asserted by others")
        }
        set(value) {
            println("The name was changed")
            field = value.toUpperCase()
        }

    var age: Int = _age
        get() = field
        set(value) {
            field = value + 10
        }
}

fun main() {
    val user = User(1, "Kildong", 30)
    user.name = ""
    println("name: $name, ${user.age}") // tempName이 null 이기 떄문에 "NONAME"만을 출력하게 된다
}
```

프로퍼티의 오버라이딩

```javascript
open class First {
    open val x: Int = 0 // 오버라이딩 가능
        get() {
            println("First x")
            return field
        }
    val y: Int = 0 // open 키워드가 없으면 final 프로퍼티
}

class Second : First() {
    override var x: Int = 0 // val -> var 부모와 구현이 다름 (반대의 경우는 불가능)
        get() {
            println("Second x")
            return field + 3
        }
        set(value) {
            field = value + 10
        }
    //override val y: Int = 0 // 에러 오버라이딩 불가
}

fun main() {
    val second = Second()
    println(second.x) // 오버라이딩 된 두번쨰 클래스 객채의 x
    second.x = 10
    println(second.x) // 23
    println(second.y) // 부모로 부터 상속 받은 값
}
```
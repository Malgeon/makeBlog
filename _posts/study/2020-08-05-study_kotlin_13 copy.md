---
layout: post
author: study
title:  "Kotlin 객체편 - [4]"
description: "Kotlin 객체 지향 프로그래밍 기본 - 4"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### 코틀린의 Properties
자바에서는 field에서 변수 선언만 가지기 떄문에 접근을 위한 메서드를 따로 만들어야 한다.
그러나 코틀린에서는 기본적으로 접근 메서드를 가지고 있다.

자바와 비교
```java
// 자바
class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setname(String name) {
        this.name = name;
    }

    public getAge() {
        return age;
    }
    ...
}
```

```javascript
// 코틀린 getter setter 메서드 생략(내부적으로 생성됨!!)
class User(_id: Int, _name: String, _age: Int) {
    val id: Int = _id
    var name: String = _name
    var age: Int = _age
}
```

```javascript
// 코틀린 좀더 간략히
class User(val id: Int, var name: String, var age: Int)
```

```javascript
fun main() {
    val user = User(1, "Sean", 30)
    // getter에 의한 값 획득
    val name = user.name
    // setter에 의한 값 지정
    user.age = 41
    println("name: $name, ${user.age}")
}
```

### Custom Getter & Setter

```
var 프로퍼티이름[: 프로퍼티자료형] [= 프로퍼티 초기화]
    [get() { getter 본문 }]
    [set() { setter 본문 }]

val 프로퍼티이름[: 프로퍼티자료형] [= 프로퍼티 초기화]
    [get() { getter 본문 }]

```

```javascript
class User(_id: Int, _name: String, _age: Int) {
    val id: Int = _id
        get() = field // 이 곳에 프로퍼티를 직접 넣으면 getter에 들어가게 되므로 무한 재귀 호출이 일어난다. 
    var name: String = _name
        get() = field
        set(value) {
            field = value
        }
    var age: Int = _age
        get() = field
        set(value) {
            field = value + 10
        }
}

fun main() {
    val user1 = User(1, "Kildong", 30)
    //user1.id = 2 // error
    user.age = 35 // setter동작
    println("user1.age = ${user1.age}") // getter 동작
}
```

value: setter의 매개변수로 외부로 부터 값을 가져온다.
 외부의 값을 받을 매개변수이므로 value 대신에 어떤 이름이든지 상관 없다.

field: 프로퍼티를 참조하는 변수로 보조 필드(backing field)로 불린다. 따라서 필드를 사용하지 않고 프로퍼티를 이용해 get을 구성한다면 무한 호출되는 재귀에 빠지게 된다. 

```javascript
class User(_id: Int, _name: String, _age: Int) {
    val id: Int = _id
    var name: String = _name
        set(value) {
            println("The name was changed")
            field = value.toUpperCase() // 받은 인자를 대문자로 변경해 프로퍼티에 할당
        }
    var age: Int = _age
}

fun main() {
    val user1 = User(1, "Kildong", 30)
    //user1.id = 2 // error
    user1.name = "coco" // 여기서 작성한 출력 코드 실행
    println("user1.name = ${user1.name}") // getter 동작
}
```

CustomGetterSetterBackingProperty
```javascript
class User(_id: Int, _name: String, _age: Int) {
    val id: Int = _id

    private var tempName: String? = null

    var name: String = _name
        get() {
            if (tempName == null) tempName = "NONAME"
            return tempName ?: throw AssertionError("Asserted by others")
        }
    var age: Int = _age
}

fun main() {
    val user1 = User(1, "kildong", 35)
    user1.name = ""
    println("user3.name = ${user1.name}") // NONAME return! tempName이 null 이기 때문에 어떤 값을 입력하더라도 NONAME return 한다.
}
```

PropertyOverride
```javascript
open class First {
    open val x: Int = 0 // 오버라이딩 가능!
        get() {
            println("Fisrt x")
            return field
        }
    val y: Int = 0 // open 키워드가 없으면 final 프로퍼티이다.
}

class Second : First() {
    override val x: Int = 0 // 부모와 구현이 다름
        get() {
            println("Second x")
            return field + 3
        }
    // override val y: Int = 0 // Error! overriding 불가!
}

fun main() {
    val second = Second()
    println(second.x) // 오버라이딩된 두번째 클래스 객체의 x
    println(second.y) // 부모로부터 상속 받은 값
}
```

```javascript
open class First {
    open val x: Int = 0
        get() {
            println("Fisrt x")
            return field
        }
    val y: Int = 0 
}

class Second : First() {
    override var x: Int = 0 // override에서 val -> var 변수 특성 또한 바꿀수 있다. (var -> val 변경은 불가능 하다!!)
        get() {
            println("Second x")
            return field + 3
        }
        set(value) { // setter override 가능!
            field = value + 10
        }
}

fun main() {
    val second = Second()
    println(second.x) 
    println(second.y) 
}

---
layout: post
author: study
title:  "Kotlin 객체편 - [8]"
description: "Kotlin 클래스 - 데이터 클래스"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 데이터 클래스

데이터 전달을 위한 객체 DTO(Data Transfer Object)
- 자바에서는 POJO(Plain Old Java Object)라고 불리기도 한다
- 구현 로직을 가지고 있지 않고 순수한 데이터 객체를 표현한다
- 데이터를 접근하는 게터/세터를 포함한다
- toString(), equals() 등과 같은 데이터 표현 및 비교 메서드를 포함한다


자바로 DTO를 포현하면 데이터 필드들, 게터/세터들, 데이터 표현 및 비교 메서드들을 모구 작성해야 한다

코틀린으로 DTO를 표현하면 프로퍼티만 신경 써서 작성하면 나머지는 내부적으로 자동 생성된다.


코틀린의 데이터 클래스(data class)
- DTO를 표현하기 적합한 클래스 표현으로 data class 키워드를 사용해 정의한다
```
코틀린의 프로퍼티 = 필드(변수) + 게터와 세터
```

자동으로 생성되는 메서드들
- 프로퍼티를 위한 게터/세터
- 비교를 위한 equals()과 키 사용을 위한 hashCode()
- 프로퍼티를 문자열로 변환해 순서대로 보여주는 toString()
- 객체 복사를 위한 copy()
- 프로퍼티에 상응하는 component1(), component2() 등


선언
```javascript
data class Customer(var name: String, var email: String)
```

데이터 클래스의 조건
- 주 생성자는 최소한 하나의 매개변수를 가져야 한다.
- 주 생성자의 모든 매개변수는 val, var로 지정된 프로퍼티여야 한다.
- 데이터 클래스는 abstract, open, sealed, inner 키워드를 사용할 수 없다.

데이터 클래스에 간단한 로직을 포함하려면 부 생성자나 init 블록을 넣어 데이터를 위한 간단한 로직을 포함 할 수 있다.
```javascript
data class Customer(var name: String, var email: String) {
    var job: String = "Unknown"
    constructor(name: String, email: String, _job: String): this(name, email) {
        job = _job
    }
    init {
        // 간단한 로직은 여기에
    }
}
```

자동 생성 메서드들
| 제공된 메서드 | 기능 |
| --- | --- | 
| equals() | 두 객체의 내용이 같은지 비교하는 연산자로 ==와 동일(고유값은 다르지만 의미값이 같을 떄) | 
| hashCode() | 객체를 구별하기 위한 고유한 정수값 생성, 데이터 셋이나 해시테이블을 사용하기 위한 하나의 생성된 인덱스 | 
| copys() | 빌더 없이 특정 프로퍼티만 변경해서 객체 복사하기 | 
| toString() | 데이터 객체를 읽기 편한 문자열로 반환하기 | 
| componentN() | 객체의 선언부 구조를 분해하기 위해 프로퍼티에 상응하는 메서드 | 


메서드 사용 예

equals() & hashCode()
```javascript
val cus1 = Customer("Sean", "sean@mail.com")
val cus2 = Customer("Sean", "sean@mail.com")
...
println(cus1 == cus2) // true
println(cus1.equals(cus2)) // true
println("${cus1.hashCode()}, ${cus2hashCode()}") // 동일한 고유값 출력
```

copy() & toString()
```javascript
var cus3 = cus1.copy(name = "Alice") // name만 변경하고자 할 때
println(cus1.toString()) // Customer(name=Sean, email=sean@mail.com)
println(cus3.toString()) //// Customer(name=Alice, email=sean@mail.com)
```

객체 디스트럭처링
- 객체가 가지고 있는 프로퍼티를 개별 변수로 분해

```javascript
val (name, email) = cus1
println("name = $name, email = $email")
```

특정 프로퍼티를 가져올 필요 없는 경우
```javascript
val(_, email) = cus1 // 첫 번째 프로퍼티 제외
```

componentN() 이용
```javascript
val name2 = cus1.component1()
val email2 = cus1.component2()
println("name = $name2, email = $email2")
```

객체 데이터가 많은 경우 for문의 활용
```javascript
val cus1 = Customer("Sean", "sean@mail.com")
val cus2 = Customer("Sean", "sean@mail.com")
val bob = Customer("Bob", "bob@mail.com")
val erica = Customer("Erica", "erica@mail.com")

val customers = listOf(cus1, cus2, bob, erica) // 모든 객체를 컬렉션 List 목록으로 구성
...
for((name, email) in customers) {
    println("name = $name, email = $email")
}
```

함수로부터 객체가 반환될 경우
```javascript
fun myFunc(): Customer {
    return Customer("Mickey", "mic@abc.com")
}
...
val (myNamem myEmail) = myFunc()
```

람다식에서 사용하는 경우
```javascript
// 람다식 함수로 Destructuring 된 변수 출력해 보기
val myLamda = {
    (nameLa, emailLa): Customer ->
    println(nameLa)
    println(emailLa)
}
myLamda(cus1)
```
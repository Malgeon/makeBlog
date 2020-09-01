---
layout: post
author: study
title:  "Kotlin 객체편 - [5]"
description: "지연초기화 - lateinit, lazy, 위임(by)"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
### 지연 초기화
변수나 객체의 값은 생성시 초기화가 필요하다. 생성 할때 초기화를 진행하기 보다 해당 정보가 나중에 정해지는 경우가 있으므로 프로그램의 성능측면에서 나중에 초기화를 진행하게 되는데 이때 해당 변수나 객체의 값은 null을 가리키게 된다. 그러나 코틀린에서는 이러한 null 값을 용납하지 못한다. 그래서 lateinit과 lazy를 이용하여 null 값을 가지지 않도록 객체의 정보가 나중에 나타나는 때에 초기화 할수 있도록 도와준다. 

### lateinit
의존성이 있는 초기화나 unit 테스트를 위한 코드를 작성시
- Car 클래스의 초기화 부분에 Engine 클래스와 의존성을 가지는 경우, 다시말해 Engine 객체가 생성되지 않으면 완전하게 초기화 할 수 없는경우
- 단위(Unit) 테스트를 위해 임시적으로 객체를 생성 시켜야 하는 경우

프로퍼티 지연 초기화
- 클래스를 선언할 때 프로퍼티 선언은 null을 허용하지 않는다.
- 하지만, 지연 초기화를 위한 lateinit 키워드를 사용하면 프로퍼티에 값이 바로 할당되지 않아도 된다.


lateinit의 제한
- var로 선언된 프로퍼티만 가능하다.
- 프로퍼티에 대한 게터와 세터를 사용할 수 없다.


```javascript
class Person {
    lateinit var name: String // 늦은 초기화를 위한 선언

    fun test() {
        if(!::name.isInitialized) { // 프로퍼티의 초기화 여부 판단 (:: -> 참조방식으로 값을 호출)
            println("not initialized") 
        } else {
            println("initialized")
        }
    }
}

fun main() {
    val kildong = Person()
    // println("name = ${kildong.name}") // UninitailizedPropertyAccessException
    kildong.test()
    kildong.name = "kildong" // 이시점에서 초기화 됨(지연 초기화)
    kildong.test()
    println("name = ${kildong.name}")
}
```

객체 지연 초기화
```javascript
data class Person(var name:String, var age: Int) 

lateinit var person1: Person // 객체의 지연 초기화

fun main() {
    person1 = Person("Kildong", 30) // 생성자 호출 시점에서 초기화 됨
    print(person1.name + " is " person1.age.toString())
}
```


### lazy
- 호출 시점에 by lazy {...} 정의에 의해 블록 부분의 초기화를 진행한다. 람다식이라 생각하면 된다.
- 불변의 변수 선언인 val에서만 사용 가능하다.(읽기 전용)
- val 이므로 값을 다시 변경 할 수 없다.

```javascript
class LazyTest {
    init {
        println("init block")
    }

    private val subject by lazy {
        println("lazy initialized")
        "Kotlin Programming" // lazy 반환 값 (람다식이라 생각하면 된다)
    }

    fun flow() {
        println("not initialized")
        println("subject one: $subject") // 이떄 초기화가 진행 된다. lazy initialized 출력.
        println("subject two: $subject")

    }
}

fun main() {
    val test = LazyTest()
    test.flow()
}
```

lazy 객체 지연 초기화
```javascript
class Person(val name: String, val age: Int)

fun main() {
    var isPersonInstantiated: Boolean = false // 초기화 용도 확인

    val person : Person by lazy {
        isPersonInstantiated = true
        Person("Kim", 23) // 객첼 반환
    }

    val personDelegate = lazy { Person("Hong", 40) } // 위임 변수를 사용한 초기화

    println("person Init: $isPersonInstantiated")
    println("personDelegate Init: ${personDelegate.isInitialized()}")

    println("person.name = ${person.name}") // 이 때 초기화
    println("personDelegate.value.name = ${personDelegate.value.name}") // 이 때 초기화

    println("person Init: $isPersonInstantiated")
    println("personDelegate Init: ${personDelegate.isInitialized()}")
}
```

by laze의 모드

3가지 모드를 지정 가능 하다
- SYNCHRONIZED 락을 사용해 단일 스레드만이 사용하는 것을 보장(기본값)
- PUBLICATION 여러 군데서 호출될 수 있으나 처음 초기화도니 후 반호나 값을 사용
- NONE 락을 사용하지 않기 때문에 빠르지만 다중 스레드가 접근할 수 있음(값의 일관성을 보장할 수 없음)

```
private val model by lazy (mode = LazyThreadSafetyMode.NONE) {
    Injector.app().transactionsModel() // 이 코드는 단일 스레드의 사용이 보장 될 때 사용하는 것이 안전하다 
}
```

### by를 통한 위임
위임(delegation)
- 하나의 클래스가 다른 클래스에 위임하도록 선언
- 위임된 클래스가 가지는 멤버를 참조없이 호출
```
< val|var|class > 프로퍼티 혹은 클래스 이름: 자료형 by 위임자
```

```javascript
interface Animal {
    fun eat() {...}
    ...
}
class Cat : Animal { }
val cat = Cat()
class Robot : Animal by cat // Amimal의 정의된 Cat의 모든 멤버를 Robot에 위임함
```

- cat은 Animal 자료형의 private 멤버로 Robot 클래스 내에 저장
- Cat에서 구현된 모든 Animal의 메소드는 정적 메소드로 생성
- 따라서, Animal에 대한 명시적인 참조를 사용하지 않고도 eat()을 바로 호출

위임을 사용하는 이유
코틀린의 기본 라이브러리는 open 되지 않은 최종 클래스
- 표준 라이브러리의 무분별한 상속의 복잡한 문제들을 방지한다.
- 단, 상속이나 직접 클래스의 기능 확장을 하기 어렵다.

위임을 사용하게 되면
- 위임을 통해 상속과 비슷하게 최종 클래스의 모든 기능을 사용하면서 동시에 기능을 추가 확장 구현할 수 있다. 


```javascript
interface Car {
    fun go(): String
}
class VanImpl(val power: String): Car{
    override fun go() = "는 짐을 적재하며 $power 마력을 가집니다."
}
class SportImpl(val power: String): Car {
    override fun go() = "는 경주용에 사용되며 $power 마력을 가집니다."
}
class CarModel(val model: String, impl: Car): Car by impl {
    fun carInfo() {
        println("$model ${go()}") // 참조 없이 각 인ㄴ터페이스 구현 클래스의 go를 접근한다.
    }
}

fun main() {
    val myDamas = CarModel("Damas 2010", VanImpl("100마력"))
    val my350z = CarModel("350Z 2008", SportImpl("350마력"))

    myDamas.carInfo() // carInfo에 대한 다형성을 나타냄
    my350z,carInfo()
}
```

by위임을 사용하지 않을 시 
```javascript
interface Car {
    fun go(): String
}
class VanImpl(val power: String): Car{
    override fun go() = "는 짐을 적재하며 $power 마력을 가집니다."
}
class SportImpl(val power: String): Car {
    override fun go() = "는 경주용에 사용되며 $power 마력을 가집니다."
}
class CarModel(val model: String, private val impl: Car): Car {
    override fun go() : String { // interface이기 때문에 override 해줘야 한다.
        return "Test" 
    }
    
    fun carInfo() {
        println("$model ${impl.go()}") // 참조 없이 각 인ㄴ터페이스 구현 클래스의 go를 접근한다.
    }
}

fun main() {
    val myDamas = CarModel("Damas 2010", VanImpl("100마력"))
    val my350z = CarModel("350Z 2008", SportImpl("350마력"))

    myDamas.carInfo() // carInfo에 대한 다형성을 나타냄
    my350z,carInfo()
}
```


### 프로퍼티 위임과 by lazy 다시보기
by lazy {...} 도 위임이다.
- 사용된 프로퍼티는 람다식 함수에 전달되어(위임되어) 함수에 의해 사용

동작분석
1. lazy 람다식 함수는 람다를 전달받아 저장한 Lazy<T> 인스턴스를 반환한다.
2. 최초 프로퍼티의 게터 실행은 lazy에 넘겨진 람다식 함수를 실행하고 결과를 기록한다.
3. 이후 프로퍼티의 게터 실행은 이미 초기화되어 기록된 값을 반환한다.


### observable과 vetoable의 위임
observable
- 프로퍼티를 감시하고 있다가 특정 코드의 로직에서 변경이 일어날 때 호출

vetoable
- 감시보다는 수여한다는 의미로 반환값에 따라 프러포티 변경을 허용하거나 취소

```javascript
import kotlin.properties.Delegates

class User {
    // observable은 값의 변화를 감시하는 일종의 콜백 루틴
    var name: String by Delegates.observable("NONAME") { // 1. 프로퍼티를 위임
        prop, old, new -> // 2. 람다식 매개변수로 프로퍼티, 기존값, 새로운 값
        println("$old -> $new) // 3 이 부분은 이벤트가 발생할 때만 실행된다.
    } // 마지막 람다식이기 때문에 중괄호를 빼내서 사용하는 것
}

fun main() {
    val user = User()
    user.name = "Kildong" // 4. 값이 변경되는 시점에서 첫 이벤트 발생
    user.name = "Dooly" // 5 값이 변경되는 시점에서 두번째 이벤트 발생
}
```

```javascript
import kotlin.properties.Delegates

fun main() {
    var max: Int by Delegates.vetoable(0) { // 1. 초기값은 0
        prop, old, new -> 
        new > old // 2. 조건에 맞지 않으면 거부권 행사
    }

    println(max) // 0
    max = 10
    println(max) // 10

    // 여기서는 기존 값이 새 값보다 크므로 false
    // 따라서 5를 재할당 하지 않는다.
    max = 5
    println(max) // 10
}
```
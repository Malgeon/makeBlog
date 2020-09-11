---
layout: post
author: study
title:  "Kotlin 객체편 - [10]"
description: "지역 클래스, 익명 객체, 실드 클래스, 열거형 클래스"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 지역 클래스
특정 메서드의 블록이다 init 블록과 같이 블록 범위에서만 유효한 클래스이며 블록 범위를 벗어나면 더 이상 사용되지 않는다.

```javascript
...
class Smartphone(val model: String) {
    private val cpu = "Exynos"
    ...

    inner class ExternalStorage(val size: Int) {
        fun getInfo() = "${model}: Installed on $cpu with ${size}Gb" // 바깥 클래스의 프로퍼티 접근
    }

    fun powerOn(): String {
        class Led(val color: String) { // 지역 클래스 선언
            fun blink(): String = "Blinking $color on $model" // 외부의 프로퍼티는 접근 가능
        }
        val powerStatus = Led("Red") // 여기에서 지역 클래스가 사용됨
        return powerStatus.blink()
    } // powerOn() 블록 끝
}

fun main() {
    ...
    val myphone = Smartphone("Note9")
    myphone.ExternalStorage(128)
    println(myphone.powerOn())
}
```

### 익명 객체

자바에서는 익명 이너 클래스라는 것을 제공해 일회성으로 객체를 생성해 사용한다.
코틀린에서는 object 키워드를 사용하는 익명 객체로 같은 기능을 수행한다.

```javascript
interface Switcher { // 인터페이스의 선언
    fun on(): String
}

class Smartphone(val model: String) {
    private val cpu = "Exynos"
    ...

    inner class ExternalStorage(val size: Int) {
        fun getInfo() = "${model}: Installed on $cpu with ${size}Gb" 
    }

    fun powerOn(): String {
        class Led(val color: String) { 
            fun blink(): String = "Blinking $color on $model" 
        }
        val powerStatus = Led("Red") 
        val powerSwitch = object : Switcher { // 익명 객체를 사용해 Switcher의 on()을 구현
            override fun on(): String {
                return powerStatus.blink()
            }
        } // 익명(object) 객체 블록의 끝
        return powerSwitch.on() // 익명 객체의 메서드 사용
    } 
}
```


### 실드(Sealed) 클래스
- 실드란 '봉인된'이라는 의미로 무언가 안전하게 보관하기 위해 묶어 두는 것
- sealed 키워드를 class와 함께 사용
- 실드 클래스 그 자체로는 추상 클래스와 같기 때문에 객체를 만들 수는 없다.
- 생성자도 기본적으로는 private이며 private이 아닌 생성자는 허용하지 않음
- 실드 클래스는 같은 파일 안에서는 상속이 가능(다른 파일에서 상속 불가)
- 블록 안에 선언되는 클래스는 상속이 필요한 경우 open 키워드로 선언

실드 클래스는 상태를 정의하고 관리하는데 주로 쓰인다.

```javascript
// 실드 클래스 선언 첫번째 스타일
sealed class Result {
    open class Success(val message: String): Result()
    class Error(val code: Int, val message: String): Result()
}

class Status: Result() // 실드 클래스 상속은 같은 파일에서만 가능
class Inside: Result.Success("Status") // 내부 클래스 상속
```
```javascript
// 실드 클래스 선언 두번째 스타일
sealed class Result

open class Success(val message: String): Result()
class Error(val code: Int, val message: String): Result()

class Status: Result()
class Inside: Success("Status")
```

```javascript
fun main() {
    /// Success에 대한 객체 생성
    val result = Result.Success("Good!")
    val msg = eval(result)
    println(msg)
}

// 상태를 검사하기 위한 함수
fun eval(result: Result): String = when(result) {
    is Status -> "in progress"
    is Result.Success -> result.message
    is Result.Error -> result.message
    // 모든 조건을 가지므로 else를 가질 필요가 없음
}
```


### 열거형 클래스
- 여러 개의 상수를 선언하고 열거된 값을 조건에 따라 선택할 수 있는 특수한 클래스
- 자료형이 동일한 상수를 나열할 수 있다(실드 클래스 처럼 다양한 자료형을 다루지 못한다.)

```
enum class 클래스이름 [(생성자)] {
    상수1[(값)], 상수2[(값)], 상수3[(값)], ...
    [; 프로퍼티 혹은 메서드] // 상수가 끝났음을 의미하는 ; 그 이후 프로퍼티 혹은 메서드를 입력한다.
}
```

```javascript
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```
```javascript
enum class DayOfWeek(val num: Int) {
    MONDAY(1), TUESDAY(2), WEDNESDAY(3), THURSDAY(4),
    FRIDAY(5), SATURDAY(6), SUNDAY(7)
}
```
```javascript
val day = DayOfWeek.SATURDAY // SATURDAY의 값을 읽기
when(day.num) {
    1, 2, 3, 4, 5 -> println("Weekday")
    6, 7 -> println("Weekend!")
}
```

```javascript
// 메서드가 포함되는 경우
enum class Color(val r: Int, val g: Int, val b: Int) {
    RED(255, 0, 0), ORANGE(255, 165, 0),
    YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238); // 여기에 세미콜론으로 끝을 알리고

    fun rgb() = (r * 250 + g) * 256 + b // 메서드를 포함할 수 있다.
}

fun main(args: Array) {
    println(Color.BLUE.rhb())
}
```

열거형 클래스 사용 예시

```javascript
interface Score {
    fun getScore(): Int
}

enum class Membertype(var prio: String) : Score { // Score를 구현할 엵형 클래스
    NORMAL("Thrid") {
        override fun getScore(): Int = 100
    },
    SILVER("Second") {
        override fun getScore(): Int = 500
    },
    GOLD("First") {
        override fun getScore(): Int = 1500
    }
}
```
```javascript
fun main() {
    println(MemberType.NORMAL.getScore()) // 100
    println(MemberType.GOLD) // GOLD
    println(MemberType.valueOf("SILVER")) // SILVER
    println(MemberType.SILVER.prio) // Second

    for (grade in MemberType.values()) { // 모든 값을 가져오기 위한 반복문
        println("grade.name = ${grade.name}, prio = ${grade.prio}")
    }
}
```

### 애노테이션 클래스
annotation
- 코드에 부가 정보를 추가하는 기능 역할
- @ 기호와 함꼐 나타내는 표기법으로 주로 컴파일러나 프로그램 실행 시간에서 사전 처리를 위해 사용
    - @Test 유닛 테스트
    - @JvmStatic 자바 코드에서 컴패니언 객체를 접근


```
annotation class 애노테이션명
```

애노테이션의 속성
- @Target: 애노테이션이 지정되어 사용할 종류(클래스, 함수, 프로퍼티 등)를 정의
- @Retention: 애노테이션을 컴파일된 클래스 파일에 저장할 것인지 실행 시간에 반영할 것인지 결정
- @Repeatable: 애노테이션을 같은 요소에 여러 번 사용 가능하게 할지를 결정
- @MustBeDocumented: 애노테이션이 API의 일부분으로 문서화하기 위해 사용

속성과 함께 정의된 애노테이션 클래스의 예

```
@Target(AnnotationTarget.CLASS, AnnotationTarget.FUNCTION,
        AnnotationTarget.VALUE_PARAMETER, AnnotationTarget.EXPRESSION)
@Retention(AnnotationRetention.SOURCE) // 애노테이션의 처리 방법 - SOURCE: 컴파일 시간에 제거됨
@MustBeDocumented
annotation class Fancy
```
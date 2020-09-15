---
layout: post
author: study
title:  "Kotlin 객체편 - [13]"
description: "제네릭 함수 혹은 메서드, 가변성의 3가지 유형"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---


## 제네릭 함수 혹은 메서드
- 해당 함수나 메서드 앞쪽에 <T>와 같이 지정
- 자료형의 결정은 함수가 호출될 때 컴파일러가 자료형 추론
- 이 자료형은 반환 자료형과 매개변수 자료형에 사용할 수 있다.

```
fun <형식 매개변수[, ...]> 함수명(매개변수: <매개변수 자료형>[, ...]): <반환 자료형>
```

```javascript
fun <T> genericFunc(arg: T): T? { ... } // 매개변수와 반환 자료형에 형식 매개변수 T가 사용된다.

fun <K, V> put(key: K, value: V): Unit { ... } // 형식 매개변수가 여러 개인 경우
```

예시
```javascript
fun <T> find(a: Array<T>, Target: T): Int {
    for (i in a.indices) {
        if (a[i] == Target) return i
    }
    return -1
}

fun main() {
    val arr1: Array<String> = arrayOf("Apple", "Banana", "Cherry", "Durian")
    val arr2: Array<Int> = arrayOf(1, 2, 3, 4)

    println("arr.indices ${arr1.indices}") // indices는 배열의 유효 범위 반환  return => arr.indices 0..3
    println(find<String>(arr1, "Cherry")) // 요소 C의 인덱스 찾아내기  return => 2
    println(find(arr2, 2)) // 요소의 인덱스 찾아내기 return => 1
}
```

형식 매개변수로 선언된 함수의 매개변수를 연산하고자 하면 람다식을 사용해야한다.
```javascript
fun <T> add(a: T, b: T): T {
    // return a + b // 오류! 자료형을 아직 결정할 수 없다.
}
```
```javascript
fun <T> add(a: T, b: T, op: (T, T)-> T): T {
    return op(a, b)
}
fun main() {
    val result = add(2, 3, {a, b -> a + b})
    val result2 = add(2, 3) {a, b -> a + b} 와 같이 표현 가능
    println(result)
}
```

형식 매개변수를 특정한 자료형으로 제한
- 자료형의 사용범위를 좁히기 위해 자료형을 제한한다.
- 자바에서는 extends나 super를 사용해 자료형을 제한한다.
- 코틀린은 콜론(:)과 자료형을 기입ㅎ면 형식 매개변수 T의 자료형이 기입한  자료형으로 제한된다.


자료형을 숫자형으로 제한하는 예시
```javascript
class Calc<T: Number> {
    fun plus(arg1: T, arg2: T): Double {
        return arg1.toDouble() + arg2.toDouble()
    }
}

fun main(args: Array<String>) {
    val calc = Calc<Int>() 
    println(calc.plus(10, 20))

    val calc2 = Calc<Double>() 
    val calc3 = Calc<Long>() 
    // val calc4 = Calc<String>()  // 에러! 제한된 자료형

    println(calc2.plus(2.5, 3.5))
    println(calc3.plus(5L, 10L))

}
```
```javascript
fun <T: Number> addLimit(a: T, b: T, op: (T, T) -> T): T {
    return op(a, b)
}
...
val result = addLimit("abc", "def", {a, b -> a + b}) 
// 제한된 자료형으로 인해 오류 발생!
```
<br>


### 상ㆍ하위 형식의 가변성

가변성(variance)이란
- 형식 매개변수가 클래스 계층에 어떤 영향을 미치는지 정의 
    - 형식 A의 값이 필요한 모든 장소에 형식 B의 값을 넣어도 아무 문제가 없다면 B는 A의 하위 형식(subtype)
    - Int는 Number의 하위 클래스


클래스와 자료형

| 형태 | 클래스인가? | 자료형인가? |
| --- | --- | --- |
| String | 네 | 네 |
| String? | 아니오 | 네 |
| List | 네 | 네 |
| List<String> | 아니오 | 네 |

하위 크래스는 상위 클래스가 수용한다.
- 하위 자료형은 상위 자료형으로 자연스럽게 형 변환이 이루어진다.

number <- Int
```javascript
val integer: Int = 1
val number: Number = integer 
// 하위 자료형 Int를 Number가 수용함 (캐스팅 되어 할당 된다.)
```

Int? <- Int
```javascript
val integer: Int = 1;
val nullableInteger: Int? - integer;
```
<br>

### 가변성의 3가지 유형

| 용어 | 의미 |
| --- | --- |
| 공변성(covariance) | T'가 T의 하위 자료형이면, C<T'>는 C<T>의 하위 자료형이다. 생산자 입장의 out 성질 |
| 반공변성(contravariance) | T'가 T의 하위 자료형이면, C<T>는 C<T'>의 하위 자료형이다. 소비자 입장의 in 성질 |
| 무변성(invariance) | C<T'>는 C<T>는 아무 관계가 없다. 생산자 + 소비자 |


![가변성의3가지유형]({{ site.baseurl }}/assets/images/study/kotlin_oop_variance.jpg)

<br><br>

#### 무변성(invariance)
- 자료형 사이의 하위 자료형 관계가 성립하지 않음
- 코틀린에서는 따로 지정해 주지 않으면 기본적으로 무변성

Any <- Int <- Nothing

```javascript
// 무변성(Invariance) 선언
class Box<T><val size: Int>

fun main(args: Array<String>) {
    // val anys: Box<Any> = Box<Int>(10) // 자료형 불일치 오류
    // val nothings: Box<Nothing> = Box<Int>(20) // 자료형 불일치 오류
}
```


#### 공변성(covariance)
- 형식 매개변수 사이의 하위 자료형 관계가 성립
- 하위 자료형 관계가 그래도 인스턴스 자료형 사이의 관계로 이어지는 경우
- out 키워드를 사용해 정의

Any <- Int <- Nothing
```javascript
// 공변성(Covariance) 선언
class Box<out T>(val size: Int)

fun main(args: Array<String>) {
    val anys: Box<Any> = Box<Int>(10) // 관계 성립으로 객체 생성 가능
    // val nothings: Box<Nothing> = Box<Int>(20) // 오류! 자료형 불일치
    println(anys.size)
}
```


#### 반공변성(contravariance)
- 자료형의 상하 관계가 반대
- 하위 클래스의 자료형을 상위 클래스의 자료형이 허용

Any <- Int <- Nothing
```javascript
// 반공변성(Contravariance) 선언
class Box<in T>(val size: Int)

fun main(args: Array<String>) {
    // val anys: Box<Any> = Box<Int>(10) // 오류! 자료형 불일치
    val nothings: Box<Nothing> = Box<Int>(20) // 관계 성립으로 객체 생성 가능
    println(nothings.size)
}
```


에시

Cat -> Animal <- Spider

```javascript
open class Animal(val size: Int) {
    fun feed() = println("Feeding...")
}
class Cat(val jump: Int): Animal(50)

class Spider(val poison: Boolean): Animal(1)

class Box1<T> // 무변성!
class Box2<out T> // 공변성!
class Box3<in T> // 반공변성!

class Box4<out T: Animal> // 공변성에 자료형 제한

// 형식 매개변수를 Animal로 제한
class Box5<out T: Animal>(val element: T) { // 주 생성자에서 val만 허용
    fun getAnimal(): T = element // out은 반환 자료형에만 사용할 수 있음
    
    
    /* 오류! T는 in 위치에 사용할 수 없음 
    즉 out이기 때문에 반환 형식으로만 사용할 수 있다.
    fun setAnimal(new: T) {
        element = new
    }
    */
}

fun main() {
    val c1 = Cat(10)
    val c2: Animal = Cat(10)
    val s1 = Spider(true)
    val s2: Animal = Spider(true)

    val a1 = Animal(20)
    val a2 = c1
    val a3: Animal = c1
    var a3: Animal = c1
    a3 = s1

    println("a1 ${a1.size} ${a1.poison}") // a1 1 true

    val b1_1 = Box1<Animal>()
    val b1_2: Box1<Animal> = Box1<Animal>
    // val b1_3: Box1<Cat> = Box1<Animal> // 에러!
    val b1_4 = Box1<Cat>()
    val b1_5: Box1<Cat> = Box1<Cat>()
    // val b1_6: Box1<Animal> = Box1<Cat>() // 에러!
    val b1_7 = Box1<Spider>()

    val b2_1 = Box2<Animal>()
    val b2_2: Box2<Animal> = Box2<Animal>
    // val b2_3: Box2<Cat> = Box2<Animal> // 에러!
    val b2_4 = Box2<Cat>()
    val b2_5: Box2<Cat> = Box2<Cat>()
    val b2_6: Box2<Animal> = Box2<Cat>() // 공변성으로 가능!
    val b2_7 = Box2<Spider>()
    val b2_8 = Box2<Int>() // 자료형이 제한되어 있지 않으므로 어떤 형이든 가능!
    val b2_9: Box2<Number> = Box2<Int>() // 상위 자료형이므로 가능!

    val b3_1 = Box3<Animal>()
    val b3_2: Box3<Animal> = Box3<Animal>
    val b3_3: Box3<Cat> = Box3<Animal> // 반공변성으로 가능
    val b3_4 = Box3<Cat>()
    val b3_5: Box3<Cat> = Box3<Cat>()
    // val b3_6: Box3<Animal> = Box3<Cat>() // 에러!
    val b3_7 = Box3<Spider>()

    val b4_1 = Box4<Animal>()
    val 4_2: Box4<Animal> = Box4<Animal>
    // val b4_3: Box4<Cat> = Box4<Animal> // 에러!
    val b4_4 = Box4<Cat>()
    val b4_5: Box4<Cat> = Box4<Cat>()
    val b4_6: Box4<Animal> = Box4<Cat>() // 공변성으로 가능!
    val b4_7 = Box4<Spider>()
    // val b4_8 = Box4<Int>() // 자료형이 제한되어서 불가능!
    // val b4_9: Box4<Number> = Box4<Int>() // 자료형 제한으로 불가능!
}
```
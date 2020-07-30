---
layout: post
author: study
title:  "Kotlin - [1]"
description: "Kotlin 기본"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---
 
## 변수
  val(value) - 불변형 (immutable)
  var(variable) - 가변형 (mutable)

  val username(: String - 컴파일에서 추론 가능할 경우 생략 가능) = "Eon"

  따라서 단순 선언 같은 경우 추론이 불가능하므로 생략 불가

## 자료형

### 기본형(primitive data type)
- 보통 코틀린에서는 사용하지 않으나 자바에서 사용하므로 알아두자
- int, long, float, double

### 참조형(reference type)
- 동적 공간에 데이터를 둔 다음 이것을 참조하는 자료형
- Int, Long, Float, Double

### 자료형 추론
123 => Int
123L => Long (접미사 L)
123F => Float (접미사 F)
0x0F => 16진수 Int (접두사 0x)
0b00001011 => 2진수 Int (접두사 0b)

작은 값을 사용하고자 하면 자료형을 명시적으로 지정해주자.

숫자 자료형을 경우 사이에 언더바는 아무런 영향을 주지 않는다. 그냥 자릿수 확인으로 사용 가능

큰수일 경우 3.14E + 16 이런식으로 사용

코틀린에서는 부동 소수점을 표현할 때, 지수부와 가수부로 나뉘어 저장된다. 이러한 부동 소수점에 대해 연산을 하게 될 경우 공간 제약으로 인해 정확한 값을 return하지 않는다. 유념해서 설계하자.

```javascript
fun main() {
  var num: Double = 0.1

  for(x in 0...999) {
    num += 0.1
  }

  println(num) //100.09999...
}
```

### 문자열 자료형

문자열은 String으로 선언되며 String Pool 이라는 공간에 구성된다. 이때 참조형 변수이기 때문에, 변수를 선언할 경우 Heap에 값은 String Pool 저장되게 되며 이 값은 변경되지 않는다. 즉, 만일 값이 같은 변수를 선언할 경우 똑같은 참조이게 된다. 

코틀린에서 [== : 값비교] [=== : 참조까지 비교]

  ```javascript
fun main() {
  var str1: String = "Hello"
  var str2 = "World"
  var str3 = "Hello"
  
  println("str1 === str2: ${str1 === str2}") // false
  println("str1 === str3: ${str1 === str3}") // true
}
```

 ### 표현식에서 문자열

   ```javascript
fun main() {
  var a = 1
  var str1 = "a = $a"
  var str2 = "a = ${a +2}"
  
  println("str1: \"$str1\", str2: \"$str2\"") // "를 print하고 싶다면 [\"] 이렇게 쓰면 된다.
}
```


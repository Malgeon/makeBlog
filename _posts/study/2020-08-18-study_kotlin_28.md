---
layout: post
author: study
title:  "Kotlin 객체편 - [18]"
description: "문자열"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 문자열 기본 처리
문자열은 불변(immutable) 값으로 생성
- 참조되고 있는 메모리가 변경될 수 없다.

```java
val hello: String = "Hello World!"

println(hello[0]) // H
//hello[0] = 'K' // Error!
var s = "abcdef"
s = "xyz" // 새로운 메모리 공간이 생성된다.
```

`기존의 문자열은 GC에 의해 제거됨`


### 문자열 추출하고 병합하기
문자열에서 특정 범위의 문자열을 추출
- substring(), subsequence()

```
String.substring( 인덱스 범위 지정 ): String
CharSequence.subSequence( 인덱스 범위 지정 ): CharSequence
```
```java
s = "abcdef"
println(s.substring(0..2)) // abc
```

추출 후 문자열 새로 할당하기

```java
var s = "abcdef"
s = s.substring(0..1) + "x" + s.substring(3..s.length-1) // ab를 추출하고 x를 덧붙이고 다시 def를 추출 -> abxdef
```

### 문자열 비교

a.compareTo(b)를 사용한 비교
- a와 b가 같다면 0을 반환하고, a가 b보다 작으면 양수, 그렇지 않으면 음수

```java
var s1 = "Hello Kotlin"
var s2 = "Hello KOTLIN"
// 같으면 0, s1<s2 이면 양수, 반대면 음수를 반환
println(s1.compareTo(s2)) // 양수
println(s1.compareTo(s2, true)) // 대소문자 무시 결과는 0
```

### StringBuilder
문자열이 사용할 공간을 좀 더 크게 잡아 사용한다.
- 간단한 요소 변경이 있을 경우 용이하다.
- 단, 기존의 문자열보다는 처리가 조금 느리며, 만일 단어를 변경하지 않는 경우 불필요한 메모리 낭비를 야기한다.
- 문자열이 자주 변경되는 경우에 사용한다.

```java
var s = StringBuilder("Hello")
s[2] = 'x' // 허용되지 않았던 요소이 변경이 가능하다. 결과는 Hexlo
```

StringBuilder 관련 메서드
- append(포함), insert(추가), delete(삭제) 등

```java
var s = StringBuilder("Hello")
s.append("World") // HelloWorld
s.insert(10, "Added") // index 10부터 추가되어 HelloWorldAdded
s.delete(5, 10) // index 5부터 10 전까지 삭제되어 HelloAdded
```

### 기타 문자열 처리

소문자/ 대문자 변경
- toLowerCase(), toUpperCase()

특정 문자 단위로 잘라내기
- split("분리문자") - 분리된 내용은 List로 반환

```java
var deli = "Welcome to Kotlin"
val sp = deli.split(" ")
println(sp) // ["Welcome", "to", "Kotlin"]
```

앞뒤 공백 제거
- trim()


### 리터럴 문자열
이스케이프(Escape) 문자

| 종류 | 의미 |
| --- | --- |
| \t | 탭(tab) |
| \b | 백스페이스(backspace) |
| \n | 개행(newline) |
| \r | 리턴(carriage return) |
| \uHHHH | 유니코드(Unicode) 문자 |
| \' | 작은따옴표(single quote) |
| \" | 큰따옴표(double quote) |
| \\ | 슬래시(slash) |
| \$ | 달러 기호(dollar) |


```java
val str = "\tYou're just Too \"good\" to be true\n\tI can't take my eyes off you."
val uni = "\uAC00" // 한글 코드의 범위 AC00-D7AF
```
```
    You're just too "good" to be true
    I can't take my eyes off you.
가
```

\r 같은 경우는 운영체제별로 다르게 동작을 할수도 있으니 유의해서 사용하자.


### 원본 문자열 그대로 나타내기

3중 따옴표 부호(""")의 이용
```java
val text = """
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
    """.trimMargin() // trim default는 |
```

### 형식 문자 사용
format()을 사용한 형식 문자

```java
inline fun String.format(vararg args: Any?): String (source)
```

| 종류 | 의미 |
| --- | --- |
| %b | 참과 거짓의 Boolean 유형 |
| %d | 부호있는 정수 |
| %f | 10진 실수 |
| %h | 해시코드 |
| %o | 8진 정수 |
| %t | 날짜나 시간 |
| %c | 문자 |
| %e | E 표기법의 실수 |
| %g | 10진 혹은 E 표기법의 실수 |
| %n | 줄 구분 |
| %s | 문자열 |
| %x | 16진 정수 |

```java
val pi = 3.1415926
val dec = 10
val s = "hello"

println("pi = %.2f, %3d, %s".format(pi, dec, s)) // 3.14, 010, hello
```
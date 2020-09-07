---
layout: post
author: test
title:  "표준 함수 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12903"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 가운데 글자 가져오기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


```java
class Solution {
    fun solution(s: String): String {
        var answer = ""
        var middle = 0
        val size = s.length
        
        var isOdd = size % 2
        var point:Int = size/2
        answer = if(isOdd == 1) s.substring(point..point) else s.substring(point-1..point)
        return answer
    }
}
```

- Int형이 아닌 수를 받으면 toInt하지 않아도 소수점 아래는 버려진다.
- 함수의 return은 String형이다. 따라서 s[1]같은 경우는 char형이기 때문에 컴파일 오류

```java
class Solution {
  fun solution(s: String) =
    with(s) { substring(length / 2 - 1 + (length % 2) .. length / 2) }
}
```

- 나의 풀이방식으로 하는 표준함수를 이용하였다. 

앞서 정리해둔 [표준 함수 활용1](../study_kotlin_9)과 [표준 함수 활용2](../study_kotlin_10)에서 자세히 다루고 있다.


사실, with와 run의 기능이 유사하기 때문에 이를 run을 이용할 수도 있다.

```java
class Solution {
    fun solution(s: String): String {
            return s.run {
                substring(length / 2 - 1 + (length % 2) .. length / 2)
            }
    }
}
```
그런데 엄밀히 따지자면 
with의 사용조건은 수신 객체가 Non-nullable이면서, 결과가 필요하지 않은 경우에 해당한다.
run은 어떤 값을 계산할 필요가 있거나 여러개의 지역 변수의 범위를 제한하기 위해 사용한다.


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12919"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 서울에서 김서방 찾기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

array에서 "Kim"을 찾아 해당 Idx를 출력해주는 문제.

나는 코틀린에 익숙하지 않아, 직접 idx를 구하고, 문자열도 합쳐주었다. 
그러나 코틀린에서는 더 나은 기능을 제공한다.

```java
class Solution {
    fun solution(seoul: Array<String>): String = 
        "김서방은 ${seoul.indexOf("Kim")}에 있다"
}
```
아래는 자바스크립트와는 다르게 콜백함수에서도 break 하여 idx를 가져오는 방법이다.

```java
class Solution {
    fun solution(seoul: Array<String>): String {
        var answer = 0

        run loop@{
            seoul.forEachIndexed{ index, value ->
                if(value.contains("Kim")) {
                    answer = index
                    return@loop
                }
            }
        }
        return "김서방은 ${answer}에 있다"
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12926"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 시저 암호</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


ASCII 값을 이용해 문제를 푸는 방식이 가장 간단하다.

kotlin은 범위 연산자와 when을 지원하므로 이것을 통해 해결했다.
추가로 [확장 함수](../study_kotlin_5)를 사용한다면 더 깔끔해진다. 

```java
class Solution {
    fun solution(s: String, n: Int): String =
        s.map {
            when (it) {
                in 'A'..'Z' -> it.calAscii(n, 'A', 'Z')
                in 'a'..'z' -> it.calAscii(n, 'a', 'z')
                else -> it
            }
        }.joinToString("")
    fun Char.calAscii(c: Int, n: Char, m: Char): Char =
        (n.toInt() + (toInt() - n.toInt() + c) % (m - n + 1)).toChar()
}
```

그런데 kotlin 제공 함수만을 이용해서 해결할 수 있다.
```java
class Solution {
    fun solution(s: String, n: Int): String {
        return String(s.toCharArray().also {
            charArray ->
            charArray.forEachIndexed {
                index, eachCharacter ->
                if(!eachCharacter.isWhitespace()) {
                    charArray[index] = eachCharacter.plus(n).run {
                        if(isLetter() && (!eachCharacter.isUpperCase() || isUpperCase())) this
                        else minus(26)
                    }
                }
            }
        })
    }
}
```
여기서 기억해야할 지점은 also의 활용인데, 위의 코딩에서 보면 String을 charArray로 변환하여 이 객체를 인자로 받아서 계산하고자 할때 가능한 방법이 let 과 also이다.

이 때 인자가 Array이기 때문에 값이 변경이 이뤄지기 때문에 해당 값을 그대로 결과값으로 return 해도 된다. 따라서 굳이 block(this)를 리턴하기 보다 this를 return 하여 block 내에서 굳이 어떤 값을 return 하도록 설계 하지 않아도 된다.

char.plus()는 char의 값을 바꾸지 않고 return한다.



<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12915"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 문자열 내 마음대로 정렬하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


커스텀 기준점으로 정렬을 하면 되는 문제이다.
기준점은 2단계로 나뉘어 있는데, 
1. 특정 순번의 Char 값을 기준으로 정렬
2. 해당 Char 값이 같을 경우, 전체 값을 기준으로 정렬


위와 같은 기준점을 해결하기 위해 방법으로는 3가지가 존재한다.

- Comparator 이용 : 가장 처음으로 떠올린 방법으로 람다식을 이용해 기준점을 설정한다.

기본적으로 람다식의 결과값이 양수면 swap, 0 또는 음수면 remain 하게 된다.
같은 값을 찾아 내야 할 필요가 없으므로, 단순히 음수로 표현하여 만들었다.
```java
class Solution {
    fun solution(strings: Array<String>, n: Int): Array<String> {
        strings.sortWith(Comparator<String> { a, b ->
            when {
                a[n] > b[n] -> 1
                a[n] == b[n] -> {
                    when {
                        a > b -> 1
                        else -> -1
                    }
                }
                else -> -1
            }
        })
        return strings
    }
}
```

- compareBy 이용 : comparator의 동작을 하나 단순하게 사용하게 도와주는 함수이다.

기준점이 한단계만 존재 할 경우, 단순히 { it.length } 와 같이 사용할 수 있다.
그러나 위 문제에선 기준점이 두단계가 존재하며, 그것은 아래와 같이 해결한다.
```java
class Solution {
    fun solution(strings: Array<String>, n: Int): Array<String> {
        var answer = strings
        var list =  answer.sortedWith(compareBy({ it[n] }, { it }))
        /*
         순서가 매우 중요한데, {it[n]} 이 같게 되면 {it} 으로 
         정렬을 해주는 것으로 이해하고있다.
        */
        return list.toTypedArray()
    }
}
```

그런데 사실 위 문제는 정렬 과정의 기준점을 단계적으로 설정하는 것과는 다르게 전체값 기준 정렬을 한 후, 특정 Char값으로 정렬을 해서 풀수 있다.

```java
class Solution {
    fun solution(strings: Array<String>, n: Int): Array<String> {
        var answer = strings
        var list =  answer.sorted()
        var result = list.sortedWith(compareBy({ it[n] }))
        return result.toTypedArray()
    }
}
```

객체의 값을 변경해도 될 경우 also와 sort()를 사용하여 해결 할 수 있다.
```java
class Solution {
    fun solution(strings: Array<String>, n: Int): Array<String> {
        return strings.also {
            it.sort()
            it.sortBy { it[n] }
        }
    }
}
```


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12918"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 문자열 다루기 기본</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

문자열을 Int로 바꿔주며, 만일 바꾸지 못할시 Null처리를 해주는 함수를 이용하여 Null시 let을 이용해 처리를 해주는 방법으로 해결하였다.

```java
class Solution {
    fun solution(s: String): Boolean {
        val len = s.length
        if(len == 4 || len == 6) {
            var isInt = s.toIntOrNull()
            isInt?.let {
                return true
            }
        }
        return false  
    }
}
```

Java에서와 같이 isDigit을 이용해 풀수 있기도 하다.
```java
class Solution {
    fun solution(s: String): Boolean {
        if (s.length != 4 && s.length != 6){
            return false
        }
        for (c in s.toCharArray()){
            if (!c.isDigit()){
                return false
            }
        }
        return true
    }
}
```
---
layout: post
author: study
title:  "Kotlin 코딩테스트 메모장"
description: "문제풀며 알아둬야할 정보들을 찾아보자."
categories: [ study ]
postImgOn: true
tags: [programming, kotlin]
image: assets/images/study/kotlin.png
insertUrlImg: assets/images/study/programmers.png
---

```
/******************************************************************************/
try {
    ...
} catch ( e: CancellationExceoption) {ㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁ
    ... 취소에 대한 예외처리 ...
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12901"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 2016년</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

내 풀이

```java
class Solution {
    fun solution(a: Int, b: Int): String {
        var answer = ""
        var monthArr = intArrayOf(31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
        var day = 0
    
        var monthCal = a-1
        var count = 0;
        while (monthCal>0) {
            day += monthArr[count]
            count++
            monthCal--
        }
        day += b-1
        var weekend = day % 7
    
        when(weekend) {
            1 -> answer = "SAT"
            2 -> answer = "SUN"
            3 -> answer = "MON"
            4 -> answer = "TUE"
            5 -> answer = "WED"
            6 -> answer = "THU"
            0 -> answer = "FRI"
        }
        return answer
    }
}
```

다른사람 풀이

```java
class Solution {
    fun solution(a: Int, b: Int): String {        
        val month = listOf(31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
        val dayOfWeek = listOf("FRI", "SAT", "SUN", "MON", "TUE", "WED", "THU")
        var diff = 0

        for (i in 0 until a - 1) {
            diff += month[i]
        }

        diff += (b - 1)

        return dayOfWeek[diff%7]
    }
}
```

- 1월 1일이 고정이라는 조건을 생각한다면, 위와 같이 단순히 for문을 통해서 월에 해당하는 일수를 더해줄 수 있다. 그리고 그런 방식의 코딩이 더 깔끔해 보이긴 하다.


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


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12910"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 나누어떨어지는 숫자 배열</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

2가지 방법을 생각하였다.
- arr에서 순환하여 조건에 맞는 값을 빈 공간에 추가하기
- arr로부터 filter를 이용해 배열을 만들기

그런데 2가지 방법 모두 Collection을 이용해야 하므로 List에서만 사용이 가능하다.
따라서 List에서 처리한후 toIntArray를 이용해 return함으로써 해결하였다.


```java
class Solution {
    fun solution(arr: IntArray, divisor: Int): IntArray {
        var answer = ArrayList<Int>()
        for (value in arr) {
            if (value%divisor == 0) {
                answer.add(value)
            }
        }
        if (answer.isEmpty()) answer.add(-1)
        return answer.toIntArray().sortedArray()
    }
}
```
```java
class Solution {
    fun solution(arr: IntArray, divisor: Int): IntArray {
        var answer = arr.filter{ it % divisor == 0 }
        if (answer.isEmpty()) return intArrayOf(-1)
        else return answer.toIntArray().sortedArray()
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12912"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 두 정수 사이의 합</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

두 정수 a, b 중 큰 값을 찾고, 순서대로 더해주는 쉬운 문제
a와 b가 같다면 굳이 for문에 넣지 말고 바로 return 해주자.

```java
class Solution {
    fun solution(a: Int, b: Int): Long {
        var answer:Long = 0
        val max:Int
        val min:Int

        if (a>b) {max=a; min=b} else if (a<b) {max=b; min=a} else return a.toLong()

        for (i in min..max) {
            answer += i
        }
        return answer
    }
}
```

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


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12917"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 문자열 내림차순으로 배치하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

단순히 문자열을 분리한 다음 내림차순 정렬 후 다시 합치면 된다.
이때, 문자열을 분리하여 저장하는 것이 CharArray와 Array<String> 두가지로 나뉠 수 있다.

- CharArray

```java
class Solution {
    fun solution(s: String): String = String(s.toCharArray().sortedArrayDescending())
}
```

- Array<String>

```java
class Solution {
    fun solution(s: String): String {
        var answer = s.map {it.toString()}.toTypedArray()
        answer.sortByDescending {it}
        return answer.joinToString("")
    }
}
```

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
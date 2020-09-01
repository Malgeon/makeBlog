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
array를 사용하게 되면 sortedArray를 사용할수 있다. array를 받아 array를 return char
또한 charArray는 String으로 감싸 String으로 return 할수 있다. (charArray, byteArray 사용 가능)

```java
class Solution {
    fun solution(s: String): String = 
        String(s.toCharArray().sortedArrayDescending())
}
```

- Array<String>
array.map은 List를 retrun하는데, 그렇게 되면 sort를 사용할수 있다. 그리고 다싯 String으로 바꾸기 위해선
joinToString을 사용해야 한다.

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

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12922"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 수박수박수박수박수박수?</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

코틀린에서 문자열연산 operator를 지원한다
즉, "연산" + "연산" => "연산연산" 

따라서 다음과 같이 해결할 수 있다.

```java
class Solution {
    fun solution(n: Int): String {
        var pattern = "수박"
        var answer = ""
        
        var isOdd = n % 2
        
        for ( x in 1..n/2) {
            answer += pattern
        }        
        if(isOdd==1) answer += pattern[0] 
        return answer
    }
}
```

- kotlin에서는 자료형의 구분이 명확하다. 즉, Int의 0, 1은 Boolean의 false, true와 다르다.


또한 문자열에서 함수를 이용해 해결이 가능하다.

```java
class Solution {
    fun solution(n: Int): String {
        return "수박".repeat(n / 2) + if (n % 2 != 0) "수" else ""
    }
}
```

CharArray을 이용해 해결이 가능하다.

```
<init>
Creates a new array of the specified size, where each element is calculated 
by calling the specified init function.

<init>(size: Int, init: (Int) -> Char)
Creates a new array of the specified size, with all elements 
initialized to null char (`\u0000').

<init>(size: Int)
```
```java
CharArray(5) // 5개 크기의 CharArray 생성
CharArray(5){ '가' } // ['가', '가', '가', '가', '가']
```

```java
class Solution {
    // fun solution(n: Int): String = String(CharArray(n, {i-> if(i%2==0) '수' else '박'}))
    fun solution(n: Int): String = String(CharArray(n){i-> if(i%2==0) '수' else '박'})
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12925"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 문자열을 정수로 바꾸기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

toInt()는 "-1234" 또한 부호화 해서 -1234로 바꿔준다.
```java
class Solution {
    fun solution(s: String) = return s.toInt()
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

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12928"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 약수의 합</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

전형적인 문제풀이

```java
class Solution {
    fun solution(n: Int): Int {
        var answer = 1
        if (n <= 1) return n
        for( i in 2..n/2) {
            if(n%i == 0) {
                answer += i
            }
        }
        return answer+n
    }
}
```

배열과 filter를 이용해서 문제를 해결할 수 있다.
이때, n 이 1일경우 (1..0.5) 이기 때문에 answer는 0이다.

```java
class Solution {
    fun solution(n: Int): Int {
        var answer = 0
        answer = (1..n/2).filter { n % it == 0 }.sum()
        return answer
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12930"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 이상한 문자 만들기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

전형적인 문제풀이

```java
class Solution {
    fun solution(s: String): String {
        var answer = ""
        var idxInWord = 0

        for (c in s) {
            if (c==' ') {
                answer += " "
                idxInWord = 0
            } else {
                idxInWord++
                if (idxInWord%2 != 0) {
                    answer += c.toUpperCase()
                } else {
                    answer += c.toLowerCase()
                }
            }
        }
        return answer
    }
}
```

mapIndexed 와 joinToString의 transform을 이용하면 문장을 확연히 줄일 수 있다.

```java
class Solution {
    fun solution(s: String): String = s.split(" ").joinToString(" ") {
        it.mapIndexed { index, value ->
            if(index%2 == 0) value.toUpperCase() else value.toLowerCase()
        }.joinToString("")
    }
}
```

joinToString의 Transform을 사용하여 각각의 항목에 대하여 mapIndexed를 적용할 수 있게 해준다.


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12931"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 자릿수 더하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


나는 문제를 보자마자 String로 변환해서 더한다음 Int로 변환해주는 것으로 설계하였다.
그러나, 사실 10으로 mod 해주면서 나머지 값을 더해주는 것도 한 방법이다. (Int가 주어지기 때문에 굳이 자릿수에 대한 생각하지 않아도 된다.)

```java
class Solution {
    fun solution(n: Int): Int {
        var input = n
        var answer = 0

        while (input != 0) {
            answer += input % 10
            input /= 10
        }
        return answer
    }
}
```

아래는 String을 이용해 문제를 해결하는 방법이다.
String.forEach가 각각의 String으로 변환해 주는 것을 기억하자.

```java
class Solution {
    fun solution(n: Int): Int {
        var res = 0
        n.toString().forEach { res += it.toInt() - '0'.toInt() }
        return res
    }
}
```

나는 위와 같이 String.forEach를 알지 못해 굳이 Char로 바꿔서 해결하였다.
Char을 사용하려니 toInt() 사용하지 못하였고, getNumericValue() 를 알게 되었다.

```java
class Solution {
    fun solution(n: Int): Int {
        var answer = 0
        n.toString().toCharArray().forEach{
            answer += Character.getNumericValue(it)
        }
        return answer
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12932"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 자연수 뒤집어 배열로 만들기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

자연수를 문자열로 만들어 reversed 한다음 다시 intArray로 만들어야 한다.

순서대로 n을 n.toString()으로 문자열을 만든다.

- 이때 해당 문자열을 split("")로 array<String>을 만들게되면
맨 앞과 맨 뒤에 공백칸이 존재하게 된다.
예를들어 1234라면 ["", "1", "2", "3", "4", ""]
해당 값들은 string이기 때문에 toInt를 하면 바로 숫자로 바뀌게 되나,
공백을 제거해 줘야 한다.

```java
class Solution {
    fun solution(n: Long): IntArray {
        return n.toString()
            .reversed()
            .split("")
            .filter { it != "" }
            .map { it.toInt() }
            .toIntArray()
    }
}
```

- 그렇다면 공백을 사용하지 않도록 map을 이용하려다 보니, 이때 다루는 값은 String의 원소
(String[0]) Char 값이다. 이것을 toInt() 이용하면 ascii값이 나온다. 따라서 기존점을 이용하여 숫자를 만들어 준다.

```java
class Solution {
    fun solution(n: Long): IntArray {
        return answer = n.toString()
            .reversed()
            .map{it.toInt() - '0'.toInt()}
            .toIntArray()
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12933"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 정수 내림차순으로 배치하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

Long을 배열로 바꾼 뒤 정렬을 하고 다시 Long으로 return 해야 하는 문제.

map으로 list<Int>로 바꿔준뒤, 정렬을 하고 다시 joinToString으로 String 변환 후 Long return

```java
class Solution {
    fun solution(n: Long): Long = n.toString()
         .map{it.toInt() - '0'.toInt()}
         .sortedDescending()
         .joinToString("")
         .toLong()
}
```

그런데 map을 쓰려는 이유가 sorted를 사용하려 했던건데, sortedArray()를 사용해서 해결도 가능하다.
sortedArray를 사용하려면 array에서만 동작하므로 toCharArray()

```java
class Solution {
    fun solution(n: Long): Long = 
        String(n.toString().toCharArray().sortedArrayDescending())
            .toLong()
}
```


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12934"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 정수 제곱근 판별</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


kotlin.Math를 사용하는 방법과 정수인지 판단하는 방법을 잘 숙지해 두자.
```
double % 1.0(double!) == 0.0(double!)
Math.pow(sqrt + 1, 2.0(double!))
```
```java
class Solution {
    fun solution(n: Long): Long {
        val sqrt = Math.sqrt(n.toDouble())
        return if(sqrt % 1.0 == 0.0) {
            Math.pow(sqrt + 1, 2.0).toLong()
        } else {
            -1L
        }
    }
}
```


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12935"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 제일 작은 수 제거하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

작은수를 찾고, 해당 index를 알아서 빼줘야하는 고민을 하고 있엇는데, filter기능을 이해하면 한번에 해결이 가능하다.

```java
class Solution {
    fun solution(arr: IntArray): IntArray {
        if(arr.size == 1) return intArrayOf(-1)
        else return arr.filter{ it != arr.min() }.toIntArray()
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12937"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 짝수와 홀수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

전형적인 문제풀이

```java
class Solution {
    fun solution(num: Int) = if(num % 2 == 0) "Even" else "Odd"
}
```

and operator를 사용하여 해결한다. 여기서 and는 논리연산 and를 의미하며 비트연산으로 판단한다.

```java
class Solution {
    fun solution(num: Int): String = if (num and 1 == 1) "Odd" else "Even"
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12940"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 최대 공약수와 최소 공배수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

```java
class Solution {
    fun solution(n: Int, m: Int): IntArray {
        val max: Int
        val min: Int
        if(n>m) {
            max = n
            min = m
        } else {
            max = m
            min = n
        }
        var gcd: Int = 1
        for(i in 2..min) {
            if(max % i == 0 && min % i == 0) {
                gcd = i
            }
        }

        return intArrayOf(gcd, m*n/gcd)
    }
}
```

```java
class Solution {
    fun solution(n: Int, m: Int): IntArray {
        val gcd = gcd(n, m)

        return intArrayOf(gcd, n * m / gcd)
    }

    tailrec fun gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
}
```
```java
class Solution {
    fun solution(n: Int, m: Int): IntArray {
        var gcd = 1
        var lcm = n * m

        for(i in Math.min(n, m) downTo 1) {
            if(n % i == 0 && m % i == 0) {
                gcd = i
                break
            }
        }

        for(i in Math.max(n, m).. lcm) {
            if(i % n == 0 && i % m == 0) {
                lcm = i
                break
            }
        }
        var answer = intArrayOf(gcd, lcm)
        return answer
    }
}
```

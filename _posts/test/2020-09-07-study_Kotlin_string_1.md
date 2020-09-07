---
layout: post
author: test
title:  "문자열 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---


### 문자열

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


<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12948"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 핸드폰 번호 가리기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

바꿀 글자 길이를 가져와서  "*"를 만든다음 substring으로 합치는 방법을 사용하였다.

```java
class Solution {
    fun solution(phone_number: String): String {
        var answer = ""
        val len = phone_number.length
        for(i in 0..len-5) answer += "*"
        return answer + phone_number.substring(len-4..len-1)
    }
}
```

slice를 사용할 수 도 있다.

```java
class Solution {
    fun solution(phone_number: String): String {
        var answer = ""
        val last = phone_number.lastIndex

        for (i in 0..last - 4) answer += "*"
        return answer + phone_number.slice(last - 3..last)
    }
}
```


### 문자열 -> 정수

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12925"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 문자열을 정수로 바꾸기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

toInt()는 "-1234" 또한 부호화 해서 -1234로 바꿔준다.
```java
class Solution {
    fun solution(s: String) = return s.toInt()
}
```

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


### CharArray or Array<String>

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

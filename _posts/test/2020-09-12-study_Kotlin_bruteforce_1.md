---
layout: post
author: test
title:  "완전탐색 : 문자열 압축"
description: "2020 KAKAO BLIND RECRUITMENT"
categories: [ test ]
tags: [ bf]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/60057"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2020 KAKAO BLIND RECRUITMENT : 문자열 압축</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


문제를 잘 살펴보면 조건이 매우 제한적이며, (abcdab 압축 시 abcdab가 되는 것이지 2abcd가 되는 것이 아니다.) 직접 해보는 방법 밖에 풀이 방법이 생각나지 않는다. 
이럴 땐 정말 직접 해봐야 된다. (이래서 이분 탐색과 완전 탐색을 파악하기 어렵게 다가온다.) 
쉽게 생각하면 효율성을 체크하지 않는 이상 오히려 완전 탐색을 하는 것이 정확할 때가 많으니 염두해 두자.

따라서 직접 구현을 해야 하는 것인데, 이는 다시 돌아와 문자열을 잘 다룰수 있는지 판단하기 위해서 나온 문제가 되는 것이다.

여러가지 방법이 있다. 
그 중에서 2가지를 제시하고자 하는데 사실, 어쨋든 문자열을 어떻게 다루는 지를 보게 되므로 풀이 방식은 상관이 없다.

1. 직접 압축된 단어를 만드는 방법
2. 압축된 단어와 해당 개수를 가진 Data 클래스를 만들어 배열로 저장하는 방법
 
직접 압축된 단어를 만드는 방법 

min 값을 얻기 위해 초기화를 시킬땐 Int.MAX_VALUE가 있다. 

```java
class Solution {
    fun solution(s: String): Int {
        var answer = 99999
        val len = s.length
        var str = s.drop(0)
        var present: String
        var count: Int
        var makeStr:String

        if(len == 1) return 1
        for(i in 1..len/2){
            present = ""
            makeStr = ""
            count = 0

            var str = s.drop(0)
            while(!(str.isNullOrEmpty())) {
                var now = str.let {
                    if (it.length < i + 1) it else it.slice(0 until i)
                }
                str = str.drop(i)

                if(present != now) {
                    var countString = if(count < 2) "" else count.toString()
                    makeStr += countString + present
                    present = now
                    count = 1
                } else {
                    count++
                }

                if(str.isEmpty()) {
                    makeStr += if(present != now) {
                        var countString = if(count < 2) "" else count.toString()
                        countString + present + now
                    } else {
                        var countString = if(count < 2) "" else count.toString()
                        countString + present
                    }
                }
            }
            answer = answer.coerceAtMost(makeStr.length)
        }
        return answer
    }
}
```

Data 클래스를 이용하는 방법
```java
class Solution {
    data class Word(
        val word : String,
        var count : Int = 1
    )
    fun solution(s: String): Int {
        var answer = Int.MAX_VALUE
        for(space in 1..s.length) {
            val compressedWordList = LinkedList<Word>()
            var startIndex = 0
            var endIndex = 0
            while(endIndex != s.length) {
                endIndex = (startIndex + space).let {
                    if(it > s.length) s.length
                    else it
                }
                val currentWord = s.substring(startIndex, endIndex)
                if(compressedWordList.isEmpty() ||
                   compressedWordList.peekLast().word != currentWord) { 
                    compressedWordList.add(Word(currentWord))
                }
                else compressedWordList.peekLast().count++
                startIndex = endIndex
            }
            val length = compressedWordList.fold(0) {
                acc, word ->
                acc + word.word.length + if(word.count == 1) 0 else {
                    word.count.toString().length
                }
            }
            if(length < answer) answer = length
        }
        return answer
    }
}
```
---
layout: post
author: test
title:  "탐욕법 : 큰 수 만들기"
description: "연습 문제"
categories: [ test ]
tags: [ greedy ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42883"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">탐욕법(Greedy) : 큰 수 만들기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


Greedy 알고리즘 문제는 얼마나 효과적인 제한 조건을 설정하고, 해당 조건을 구현할수 있는 지를 판단하는 것 같다.

이 문제 또한 그러한데, 자릿 수가 정해져 있기 때문에 빼고자 하는 숫자의 자리의 범주를 정할수 있고, 그 숫자를 빼면서 진행해 가면된다.

여기서 문제 해결을 위한 두가지 방법이 있다.
1. 정해지는 자리의 범주를 지정하고 그 범주 내의 max값의 index를 구하는 방법.
    이것은 다시 두가지 갈래로 방법이 나뉜다.
    - 큰 수로 만들어지도록 숫자를 제거하면서 진행
    - 큰 수로 만들어지는 숫자를 뽑으면서 진행
2. 스택을 사용. 
    자릿 수에 의해 자리의 범주가 정해지는데, 스택을 사용한다면 낮은 자리 부터 검사가 진행되도록 강제할수 있게 된다. 

1번 에서 큰 수로 만들어지도록 숫자를 제거하면서 진행을 하게 되면 치명적인 문제점이 존재하는데, 대상 문자의 길이와 제거하고자 하는 글자 수의 차이가 결국 같아져서 drop(-1)을 입력하게 된다.
이 경우를 추가적으로 제외해 줘야 한다.

만약 문제로 나와서 해결하다보면 문제점을 결국에는 찾지 못하리라고 생각이 드는 지점.

##### 1. 자리의 범주를 지정

- 숫자를 제거하면서 진행

```java
class Solution {
    fun solution(number: String, k: Int): String {
        return makeBigNum(number, k)
    }
    private fun makeBigNum(number: String, k: Int, fixedStr: String = "" ): String {
        if(k == 0) return fixedStr + number
        if(number.length-1 == k) return fixedStr + number.maxBy { it.toInt()}.toString() 
        // 추가 조건
        var choiceStr = number.dropLast(number.length - k-1)
        val dropIdx: Int = findIndex(choiceStr)
        return if (dropIdx == 0)  makeBigNum(number.drop(1), k, fixedStr + number.first()) 
               else makeBigNum(number.drop(dropIdx), k-dropIdx, fixedStr)
    }

    private fun findIndex(choiceStr: String): Int {
        // 추가 함수
        var max = 0
        var idx = 0
        for(i in choiceStr.indices) {
            var value = choiceStr[i].toInt()
            if(max < value ) {
                max = value
                idx = i
            }
        }
        return idx
    }
}
```

- 숫자를 뽑으면서 진행

여기서 생각해 볼만한 지점은 아래와 같다.

1. 재귀로 풀게되면 수행 시간이 늘어가는 것으로 판단이 된다.
아래의 코드에 findIndex는 단순히 max index를 찾는 함수인데, 이를 indexOf와 maxBy로 해결하게 되면 시간 초과가 나오게 된다.
반복문으로 하게 되면 비슷한 매커니즘으로 동작하는 문제이면서, indexOf와 max를 사용했음에도 절반의 속도의 향상이 있었다.
2. 어떤 결과값이 null일 수도 있는 경우 kotlin에서는 해당 null 처리를 꼭 하도록 요구를 하는데, 이를 let으로 쉽게 처리가 가능하다.
잘 숙지해두자.

```java
class Solution {
    fun solution(number: String, k: Int): String {
        var answer = ""
        var stringSize = number.length - k
        var index = 0
        val list = mutableListOf<Char>()
        while (stringSize > 0) {
            val string = number.substring(index, number.length - (stringSize - 1))
            string.max()?.let {max ->
                index += string.indexOf(max) + 1
                list.add(max)
            }
            stringSize -= 1
        }
        answer = list.joinToString("")
        return answer
    }
}
```

array로 만들어서 with를 사용해 풀수도 있다.

```java
class Solution {
    fun solution(number: String, k: Int): String {
        val answerBuilder = StringBuilder()
        with(number.toCharArray()) {
            var remainLetterCount = number.length - k
            var left = 0
            while(remainLetterCount != 0) {
                if(remainLetterCount == size - left) {
                    for(eachIndex in left .. size - 1) answerBuilder.append(this[eachIndex])
                    return answerBuilder.toString()
                }
                var frontSubMaxValue = '/'
                var frontSubMaxIndex = -1
                for(eachIndex in left .. size - remainLetterCount) {
                    if(frontSubMaxValue < this[eachIndex]) {
                        frontSubMaxValue = this[eachIndex]
                        frontSubMaxIndex = eachIndex
                    }
                }
                answerBuilder.append(frontSubMaxValue)
                remainLetterCount--
                left = frontSubMaxIndex + 1
            }
        }
        return answerBuilder.toString()
    }
}
```


##### 2. 스택을 사용

stack에서도 joinToString()을 사용할 수 있다.
만약 자릿수에 따라 크기가 결정이 되었는데도, 빼야할 숫자가 여전히 남아 있다면
이미 정렬이 되어 있으므로 뒤에서 부터 빼면 된다. 즉 그냥 앞에서 부터 substring 하면 된다.

```java
class Solution {
    fun solution(number: String, k: Int): String{
        var stack = Stack<Char>()
        var count = 0

        for( i in number.indices) {
            if(stack.isEmpty()) {
                stack.push(number[i])
                continue
            }
            while(stack.peek() < number[i]) {
                stack.pop()
                count++
                if(count == k) return stack.joinToString("") + number.substring(i, number.length)
                if(stack.isEmpty()) break
            }
            stack.push(number[i])
        }
        return stack.joinToString("").substring(0, number.length-k)
    }
}
```


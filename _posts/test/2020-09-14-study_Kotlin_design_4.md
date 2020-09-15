---
layout: post
author: test
title:  "설계 : 괄호변환"
description: "2020 KAKAO BLIND RECRUITMENT"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/60058"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2020 KAKAO BLIND RECRUITMENT : 괄호 변환</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


주어진 조건을 가지고 설계를 해낼수 있는지 파악하기 위함이 목적이라고 생각이 된다.
여기서 시간을 많이 잡아먹지 않도록 해야 하는데, 그러기 위한 방법은 하나 뿐이라고 생각한다.
정말 여러문제를 많이 풀어봐야 한다.
빠른 시간내에 해결하려면 문자열, 배열, List, map 등 을 다룰때 조건에 맞게 사용할 수 있는 각종 함수와 언어의 제약 조건을 잘 파악해야 하는데,
이것은 단순한 암기가 아닌 여러 문제를 풀면서 나오는 노하우라고 할수 있기 때문이다.

풀이 과정은 특별할 것 없이 지문에 맞는 기능을 구현 했는지 확인하면서 설계해 나가면 된다.
이때 주의할 점은 지문의 요구를 정확하게 파악하고 있어야 한다.

아래의 답안에서 reverseStr을 reversed()를 사용하지 않고 map을 사용한 이유는 지문에서의 요구가 역순으로 정렬하는 것이 아닌
해당 값을 뒤집는 것이기 때문이다. 여기서 틀린 사람들이 많을 것으로 예상 된다.

```java
class Solution {
    fun solution(p: String): String {
        return if(checkCollect(p)) p else makeCollect(p)
    }

    private fun makeCollect(p: String): String {
        val balanceIdx = checkBalance(p)
        return if(balanceIdx >= 0) {
            var subStr = p.substring(0 until balanceIdx)
            if (checkCollect(subStr)) subStr + makeCollect(p.drop(balanceIdx))
            else "(" + makeCollect(p.drop(balanceIdx)) + ")" + reverseStr(subStr)
        } else ""
    }

    private fun reverseStr(subStr: String): String {
        return if(subStr.length <= 2) "" else  subStr.drop(1).dropLast(1).map { if(it == '(') ')' else '(' }.joinToString()
    }

    private fun checkCollect(subStr: String): Boolean {
        val stack = Stack<Char>()
        val first = subStr.first()
        if(first == ')') return false
        stack.push(first)
        var flag = true
        for(i in 1 until subStr.length) {

            when {
                stack.isEmpty() -> {
                    if(subStr[i] == ')')return false else stack.push(subStr[i])
                }
                stack.peek() == subStr[i] -> {
                    stack.push(subStr[i])
                }
                else -> stack.pop()
            }
        }
        return stack.isEmpty()
    }

    private fun checkBalance(p: String): Int {
        if( p == "") return -1

        val stack = Stack<Char>()
        stack.push(p.first())
        var count = 1

        for(i in 1 until p.length) {
            if(stack.isEmpty()) return count
            var compare = stack.peek()
            if( compare == p[i] ) stack.push(p[i])
            else stack.pop()
            count++
        }
        return count
    }
}
```
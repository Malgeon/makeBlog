---
layout: post
author: test
title:  "2020 카카오 인턴십 : 수식 최대화"
description: "설계"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/67257"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2020 카카오 인턴십 : 수식 최대화</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

아마도, 대학 전공 과목에서 계산기 예제를 많이 다루기 때문에 문제 분류가 2 Level로 구분되어 있다고 생각한다.

여러문제 풀이방법이 있겠지만, 여기서는 주어진 문자열을 그대로 숫자와 연산자로 나뉘어 배열로 저장한 뒤
연산자 우선순위에 따라 연산을 병렬적으로 진행하도록 하였다.


```java
class Solution {
    private val order = listOf('+', '-', '*')
    val orderList = listOf(listOf('+', '-', '*'), listOf('+', '*', '-'), listOf('-', '+', '*'), listOf('-', '*', '+'), listOf('*', '+', '-'), listOf('*', '-', '+'))

    fun solution(expression: String): Long {
        var answer: Long = 0
        val strList = mutableListOf<String>()
        var temp = ""
        expression.forEachIndexed { idx, c ->
            if(c.isDigit()) temp+=c
            else{
                strList.add(temp)
                strList.add(c.toString())
                temp=""
            }
        }
        strList.add(temp)
        orderList.forEach {
            answer = max(answer, abs(execute(strList,it)))
        }
        return answer
    }

    fun operatorDef(a: String, b: String, op: Char): Long {
        return when (op) {
            '+' -> {
                a.toLong() + b.toLong()
            }
            '-' -> {
                a.toLong() - b.toLong()
            }
            '*' -> {
                a.toLong() * b.toLong()
            }
            else -> return 0L
        }
    }

    fun execute(str: List<String>, opList: List<Char>):Long {
        var tmp = str
        opList.forEach {
            tmp = calc(tmp, it)
        }
        return tmp.joinToString ("").toLong()
    }

    fun calc(s: List<String>, op: Char): List<String> {
        val rtn = mutableListOf<String>()
        var flag = false

        for(i in s.indices) {
            if(flag) {
                flag = false
                continue
            }
            if(!order.contains(s[i].first())) {
                rtn.add(s[i])
            } else {
                if(s[i].first() == op) {
                    flag = true
                    val before = rtn.last()
                    rtn.removeAt(rtn.size-1)
                    rtn.add(operatorDef(before, s[i+1], op).toString())
                }
                else {
                    rtn.add(s[i])
                }
            }
        }
        return rtn
    }
}
```

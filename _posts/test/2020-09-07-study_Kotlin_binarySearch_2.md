---
layout: post
author: test
title:  "이분 탐색 : 가사 검색"
description: "2020 KAKAO BLIND RECRUITMENT"
categories: [ test ]
tags: [ binary ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/60060"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2020 KAKAO BLIND RECRUITMENT : 가사 검색</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


이분 탐색이라고 타이틀을 정했지만, 사실 이 문제로부터 이분 탐색을 끌어내는 것 보다 더 쉽게 풀수 있는 방법이 Trie 자료구조라는 것이다.
그럼에도 이분 탐색이라고 하는 이유는 Trie 자료구조라는 것은 실무에서 적용 사례가 드물기 때문에, 만일 필요하다면 그때 가서 공부하는 것이 더욱 좋을 것 같은 판단 떄문이다.

정답률은 0.8%로, 아마도 문제로 확인하고 나서야 이러한 자료구조라는 것이 있다는 것을 사람들이 많이 생각했을것 같다.

심지어 카카오에서도 풀이 방법으로 제시한 것이 Trie구조 이지만, 이분 탐색으로도 풀수 있다고 첨언을 해놓았기 때문에 앞서 밝힌 생각을 가지고 이분 탐색으로 풀고자 한다.


이 문제 또한 문제 설명을 토대로 구현을 하다보면 결국에는 완전탐색으로 만들어지게 된다.

문제를 살펴보면 queries에 있는 값이 words와 비교해 특정 조건이 같은 값의 개수를 배열로 return을 해야한다.  <Br>
따라서 값을 하나마다 조건에 대입해 검사를 하고 해당 개수를 count하는 방법으로 해결할 수 있는데, 이는 완전탐색이다. <Br>

아래가 위의 방법으로 풀어본 예시 <br>
그나마 효율성에서 어느정도 점수를 받기 위하여 data클래스를 사용하여, length 비교롤 추가시켰다.

```java
data class QuerySet(var length: Int, var frontFlag: Boolean, var markNum: Int, var leftStr: String)
class Solution {
    fun solution(words: Array<String>, queries: Array<String>): IntArray {
        var answer = IntArray(queries.size) { 0 }
        val querySet = makeQuerySet(queries)

        querySet.forEachIndexed{ index, query ->
            words.forEach {
                if (queryCompare(it, query)) answer[index]++
            }
        }
        return answer
    }

    fun makeQuerySet(queries: Array<String>): List<QuerySet> {
        var querySet = mutableListOf<QuerySet>()

        queries.forEach {
            val len = it.length
            var queryInfo = QuerySet(len, false, 0, "")
            if(it[0] == '?') {
                queryInfo.frontFlag = true
                var num = getMarkNum(it, true)
                queryInfo.markNum = num
                queryInfo.leftStr = it.reversed().substring(0 until len-num)
            } else {
                queryInfo.frontFlag = false
                var num = getMarkNum(it, false)
                queryInfo.markNum = num
                queryInfo.leftStr = it.substring(0 until len-num)
            }
            querySet.add(queryInfo)
        }
        return querySet
    }

    private fun getMarkNum(query: String, frontFlag: Boolean): Int {
        var count = 1
        if(frontFlag) {
            for( i in 1 until query.length) {
                if(query[i] == '?') count++
                else return count
            }
        } else {
            for( j in query.length-2 downTo 0) {
                if (query[j] == '?') count++
                else return count
            }
        }
        return -1
    }

    private fun queryCompare(oneWord: String, query: QuerySet): Boolean {
        if(query.length != oneWord.length) return false
        return if(query.frontFlag) {
            query.leftStr.compareTo(oneWord.reversed().substring(0 until oneWord.length-query.markNum)) == 0
        } else {
            query.leftStr.compareTo(oneWord.substring(0 until oneWord.length-query.markNum)) == 0
        }
    }
}
```

- 결과 : 정확성 100% 효율성 60%

위와 같은 이해를 바탕으로 더 빨리 풀수 있는 방법(스킬)을 찾아야 하는데, 결론적으로 찾은 방법은 이분탐색이다.

방법을 구체적으로 설명하자면
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
testFeatured: true
hidden: true
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/60060"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2020 KAKAO BLIND RECRUITMENT : 가사 검색</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


이분 탐색이라고 타이틀을 정했지만, 사실 이 문제로부터 이분 탐색을 끌어내는 것 보다 더 쉽게 풀수 있는 방법이 있는데 Trie 자료구조라는 것이다. <br>
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
1. words를 아래와 같은 조건으로 정렬을 한다.
    - 우선 length로 정렬을 하고, length가 같으면 문자열 순 정렬
2. words의 요소들을 reverse를 하고 똑같은 방법으로 정렬을 한다.
3. 이분 탐색이므로 값을 먼저 예상을 하고 해당 값이 잎서 정리한 값을 찾기위한 특정한 조건에 일치하는지를 검사한다.
4. 구하고자 하는 값은 조건에 맞는 값의 개수이므로 3번에서의 일치하는 값 중에서 처음 값(lower bound)과 마지막 값(upper bound)의 index를 구한후 두 index를 빼주면 조건에 맞는 값의 개수가 나온다.

아래가 해당 방법으로 푼 예시 <br>
아래에선 값을 예상할 때, 위에서 완전탐색으로 푼 문제를 통해 결국에는 값이 stones에서 나올수 밖에 없기 때문에 stones의 중복값을 제거하고 정렬을 한 뒤
index 이분탐색을 진행하였다. 물론 러프하게 이분탐색을 진행해도 된다.

사실 기본 upper/lower bound의 컨셉은 양끝지점으로 부터 절반씩 지점으로 옮기며 찾아가는 방식인데, 이 방식은 양끝지점이 한번 정해지면 해당 범주를 설계상 전혀 넘어갈 수 가 없다. 그렇기 때문에 이 문제에서 만일 구하고자 하는 값의 right값이 마지막 값이거나, 초기값일 경우에 대해 구하지 못하게 된다.

그래서 다른 방법을 찾아보니 c++에서 직접 설계를 해놓은 [upper_bound](http://www.cplusplus.com/reference/algorithm/upper_bound/)와 [lower_bound](http://www.cplusplus.com/reference/algorithm/lower_bound/)를 찾게 되었고 이 방식은 step을 통한 전진 후진 방식이므로 mid 값을 구하는 과정에서 범주를 자유롭게 넘나들수 있게 된다. 

그래서 참고하여 만들었다.
주의해야할 사항은 compare에서 pivot과 target의 위치가 upper_bound 와 lower_bound에서 각각 달라야 한다.

```java
class Solution {
    fun solution(words: Array<String>, queries: Array<String>): IntArray {
        var answer = IntArray(queries.size) { 0 }
        val sortedWords = words.sortedWith(compareBy({it.length}, {it}))
        val sortedReverseWords = words.map { it.reversed() }.sortedWith(compareBy({it.length}, {it}))

        var wordStart = 0
        var wordEnd = words.size-1
        var len:Int
        var left: Int
        var right: Int

        queries.forEachIndexed{ index, item ->
            len = item.length

            if(item[0] == '?') {
                var pivot = item.reversed()
                var leftPivot = pivot.map { if(it == '?') 'a' else it}.joinToString(separator = "")
                var rightPivot = pivot.map { if(it == '?') 'z' else it}.joinToString(separator = "")
                left = lowerBound(sortedReverseWords, wordStart, wordEnd, leftPivot)
                right = upperBound(sortedReverseWords, wordStart, wordEnd, rightPivot)
            } else {
                var leftPivot = item.map { if(it == '?') 'a' else it}.joinToString(separator = "")
                var rightPivot = item.map { if(it == '?') 'z' else it}.joinToString(separator = "")
                left = lowerBound(sortedWords, wordStart, wordEnd, leftPivot)
                right = upperBound(sortedWords, wordStart, wordEnd, rightPivot)
            }
            answer[index] = right-left
        }
        return answer
    }

    private fun lowerBound(list: List<String>, start: Int, end: Int, pivot: String): Int {
        var target: String
        var step: Int
        var advance: Int
        var first = start
        var count = end - start + 1

        while(count > 0) {
            step = count/2
            advance = first+step
            target = list[advance]
            if(queryCompare(target, pivot)) {
                // pivot과 target의 순서 중요!
                first = ++advance
                count -= step + 1
            } else {
                count = step
            }
        }
        return first
    }

    private fun upperBound(list: List<String>, start: Int, end: Int, pivot: String): Int {
        var target: String
        var step: Int
        var advance: Int
        var first = start
        var count = end - start + 1

        while(count > 0) {
            step = count/2
            advance = first+step
            target = list[advance]
            if(!queryCompare(pivot, target)) { 
                // pivot과 target의 순서 중요!
                first = ++advance
                count -= step + 1
            } else {
                count = step
            }
        }
        return first

    }
    
    private fun queryCompare(a: String, b: String): Boolean {
        if(a.length < b.length) return true
        else if(a.length == b.length) {
            if(a < b) return true
        }
        return false
    }
}
```
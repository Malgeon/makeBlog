---
layout: post
author: study
title:  "Kotlin 코딩테스트 템플릿"
description: "문제를 풀때 참고할만한 정보"
categories: [ study ]
postImgOn: true
tags: [programming, kotlin]
image: assets/images/study/kotlin.png
---

```
/******************************************************************************/
try {
    ...
} catch ( e: CancellationExceoption) {ㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁㅁ
    ... 취소에 대한 예외처리 ...
}
```

# 테스트 환경 설정

코틀린 환경으로 문제를 풀게 될 경우 풀이를 할 환경은 다음과 같이 제시되어 있다.

```java
// IntArray와 2차 IntArray를 매개변수로 받아 IntArray를 Return하는 문제.
class Solution {
    fun solution(array: IntArray, commands: Array<IntArray>): IntArray {
        var answer = intArrayOf()
        return answer
    }
}
```
이때, 디버깅을 하기 위해 IDE 환경에서 풀고자 한다면 아래와 같이 설정하도록 하자.

```java
fun main() {
    val array = intArrayOf( ... )
    val commands = arrayOf(intArrayOf( ... ), ... )

    val test = Solution()
    println(Arrays.toString(test.solution(array, commands)))
}
```


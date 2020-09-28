---
layout: post
author: test
title:  "완전탐색 : 카펫"
description: "연습문제"
categories: [ test ]
tags: [ bf ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/42842"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">완전탐색 : 카펫</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


우선 yellow의 약수를 x, y라고 하자.
제한조건에서 brown이 정확하게 들어온다고 가정할 시, brwon은 2(x+y)+4가 될 것이다. 이때 주어진 brown과 값이 같다면 끝.

주의해야 할 것은 yellow를 기준으로 값을 찾을 때, 찾는 범주이다. 


```java
class Solution {
    fun solution(brown: Int, yellow: Int): IntArray {
        var answer = intArrayOf(0, 0)
        var x:Int = 0
        var y:Int = 0
        for( i in 1 .. yellow) {
            if(yellow%i==0) {
                x = i
                y = yellow/i
                var brownNum = 2*(x+y)+4
                if(brownNum == brown) break
            }
        }
        answer[0] = y+2
        answer[1] = x+2
        return answer
    }
}
```

위의 풀이를 아래와 같이 더욱 간단하게 풀 수 있다.

```java
class Solution {
    fun solution(brown: Int, yellow: Int): IntArray {
        return (1..yellow).filter { yellow % it == 0 }
                .find{ brown == (yellow / it * 2) + (it * 2) + 4}!!
                .let { intArrayOf(yellow / it + 2, it + 2)}
    }
}
```

이때 find의 return형은 Int?이므로 return이 IntArray?로 만들어 진다.
그러나 문제에서는 return이 IntArray여야 하므로, 해당 조건이 들어오도록 설정을 해야 한다.

똑같은 해결 방법이지만 first를 사용하면 first의 람다식에서 null이 발생되면 Error가 되어 버리는 위와 똑같이 동작을 한다.

```java
class Solution {
    fun solution(brown: Int, yellow: Int): IntArray {
        return (1..yellow)
                .filter { yellow % it == 0 }
                .first { brown == (yellow / it * 2) + (it * 2) + 4}
                .let { intArrayOf(yellow / it + 2, it + 2)}
    }
}
```




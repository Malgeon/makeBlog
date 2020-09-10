---
layout: post
author: test
title:  "연습 문제 : 멀쩡한 사각형"
description: "Summer/Winter Coding(2019)"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/62048"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">Summer/Winter Coding(2019) : 멀쩡한 사각형</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


우선 서로소인 사각형의 대각선을 생각해 보자.

서로소인 관계를 설정하는 이유는, 그렇지 않을 때 대각선은 나뉘어지는 부분이 있기 때문이다.

예를들어 5X4인 사각형(서로소인 관계의 사각형)의 대각선이 칠해진 개수는 

□□□■■
□□■■□
□■■□□
■■□□□

□□□□■
□□□■■
□□■■□
■■■□□ 

□□□□■
□□□□■
□□□■■
■■■■□

□□□□■
□□□□■
□□□□■
■■■■■

5+4-1 = 8개

가로 + 세로 -1 이 대각선에 의해 칠해진 개수가 된다.

그렇다면 m × n 사각형에서 m과 n의 최대공약수가 a라고 할 떄, m = ab, n = ac 라고 할 수 있다.(b와 c는 서로소)

그렇게 되면 m × n의 사각형의 넓이는 a² × bc 이며 이는 b × c의 사각형이 a²개가 있다고 생각할 수 있다.

이 때, b × c의 사각형을 1 × 1 의 사각형으로 치환을 한다면, a × a 의 정사각형이 있다고 할수 있으며, 
a × a 정사각형의 대각선이 지나가는 사각형의 개수는 a개가 된다.

따라서 b × c의 사각형에서 대각선에 의해 칠해진 사각형 개수는 b+c-1 이며
이는 다시 m × n의 사각형의 경우 a(b+c-1)가 된다.

a(b+c-1) = ab+ac-a 이므로 [가로 + 세로 - 가로와 세로의 최대공약수]가 대각선이 칠해진 사각형의 개수이다. 


```java
class Solution {
    fun solution(w: Int, h: Int): Long {
        var gcd: Int = 1
        for(i in Math.min(w, h) downTo 2) {
            if(w % i == 0 && h % i == 0) {
                gcd = i
                break
            }
        }      
        return w.toLong()*h.toLong()-w.toLong()-h.toLong()+gcd.toLong()
        //숫자는 모르니 한번에 바꾸기 보다 부분별로 다루도록 하자.
    }
}
```

수학적 사고력을 요하는 문제.



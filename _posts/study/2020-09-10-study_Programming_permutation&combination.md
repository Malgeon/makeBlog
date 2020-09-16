---
layout: post
author: study
title:  "순열과 조합"
description: "순열과 조합을 만들어보자"
categories: [ study ]
tags: [programming, kotlin]
postImgOn: false
image: assets/images/study/kotlin.png
---


### 순열과 조합

순열과 조합은 여러 군데에서 쓰인다.
쓰이는 방법도 다양한데
- 각각의 배열의 값으로써
- 조합된 원소의 값들로써

하지만 설계하는 것이 즉석에서 곧바로 할 수 있는 일이 아니기 때문에, 미리 이해하는 차원에서 만들어 놓고자 한다.

### 결과값 구하기

nPr => n개의 경우를 r 번 사용할 경우
nCr -> nPr에서 중복 원소를 제외한 개수

- nPr

```java
fun permutaion(n: Int, r: Int): Int {
    var value = 1
    
    for(i in 0 until r) {
        value *= n-i
    }
    return top/bottom
}
```

- nCr

```java
fun combination(n: Int, r: Int): Int {
    var top = 1
    var bottom = 1

    for(i in 0 until r) {
        top *= n-i
        bottom *= r-i
    }
    return top/bottom
}
```

### 조합된 원소 구하기

- nPr


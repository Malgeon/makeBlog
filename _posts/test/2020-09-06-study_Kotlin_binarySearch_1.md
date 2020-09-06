---
layout: post
author: test
title:  "이분 탐색 : 징검다리 건너기"
description: "2019 카카오 개발자 겨울 인턴십"
categories: [ test ]
tags: [ stack, queue ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/64062"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">2019 카카오 개발자 겨울 인턴십 : 징검다리 건너기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.] = 완전탐색으로 풀면 0 점이라는 소리다.
보통 이런 문제의 가장 큰 해결 방안 중 하나는 이분 탐색이다.


그렇지만 문제에서 설명하는 대로 문제를 해결하다 보면 방법이 결국에는 완전탐색쪽으로 떠오른다.
이 문제를 살펴보면 stone의 값이 결국 걸어갈 수 있는 인원을 나타내므로 stone의 가장 적은 값부터 빼주되 처음 0이하가 되는 index를 true로 하는 배열을 가질 수 있다.
이 배열에서 true를 가지는 index의 연속적인 개수가 k값보다 커지는 직후의 순간을 찾아내도록 하는 방법이 하나의 방법이 될수 있는데, 이는 완전탐색이다.

```java

```


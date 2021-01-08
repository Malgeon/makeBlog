---
layout: post
author: study
title:  "Kotlin - by 1. Property Delegation (+ by lazy)"
description: "코틀린의 Property Delegation 동작 분석 (+ 지연 초기화 분석)"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 개요

안드로이드를 공부하다 보면, viewModel을 할당하는 부분에서 by가 쓰이는 것을 볼수 있다.
여기선 왜 by가 쓰이는지, [단순히 by를 소개했던 지난 포스팅](../study_Kotlin_15)보다 자세히 다루려고 한다.

코틀린에서는 두 가지의 Delegation을 소개한다.
[Property Delegation](https://kotlinlang.org/docs/reference/delegated-properties.html)과 [Implementation by Delegation](https://kotlinlang.org/docs/reference/delegation.html)

두가지 모두 by를 사용하지만, 기능이 다르므로 나눠서 설명하겠다.

### 작성중 (작성날짜는 편의상 작성하기 시작한 날입니다.. 완료한 날이 아니에요 ㅎㅎ)

[참고자료 1](https://medium.com/til-kotlin-ko/kotlin-delegated-property-by-lazy%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80-74912d3e9c56)

<br><br>

[참고자료 2](http://blog.naver.com/PostView.nhn?blogId=yuyyulee&logNo=221380919035&categoryNo=22&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search&userTopListOpen=true&userTopListCount=5&userTopListManageOpen=false&userTopListCurrentPage=1)

---
layout: project
author: project
title:  "Movie Clone Application"
image: assets/images/project/android/main.JPG
description: 간단한 API를 이용하여 영화 앱을 만들어보자.
categories: [ project ]
tags: [ android ]
project: true

maindescriptions: [ "부스트코스 안드로이드 과정 중 영화 앱 프로젝트에서 주어진 API를 이용하여, 해당 API의 정보를 보여주는 영화 어플리케이션입니다.", "디자인 패턴은 MVVM, Database는 Room, viewModel data binding은 LiveData를 적용하였습니다." ]
system: [ "개발 환경: Android Studio", "Spec : kotlin, MVVVM, koin, jxJava, retrofit, Room, coroutine", "역할 : 개발" ]
developperiod : "2020.11.30 ~ 2020.01.25"
link: "Github"
linkUrl: https://github.com/Malgeon/MovieAppPractice
imagenum: 3
images: [ assets/images/project/android/main.JPG, assets/images/project/android/content.JPG, assets/images/project/android/rating.JPG ]
---

# SPEC

### 앱 아키텍처

아래와 같이 안드로이드 개발자 페이지에서 추천하는 아키텍쳐로 구성하였다.

![final-architecture]({{ site.baseurl }}/assets/images/project/android/final-architecture.png)

### 디자인 아키텍쳐 : MVVM

### DI - Koin

다음 프로젝트에서는 Dagger-Hilt를 사용할 예정이다.

### NetWork - rxJava, retrofit

![android!]({{ site.baseurl }}/assets/images/project/android/android.jpg)

<br><br>

# 리팩토링 관련 자료

### LinearLayout -> ConstraintLayout

[ConstraintLatout](https://academy.realm.io/kr/posts/constraintlayout-it-can-do-what-now/)에 관한 글.

### ListAdapter

[ListAdapter](https://zion830.tistory.com/86)에 관한 글.

### Lifecycle

[Lifecycle](http://pluu.github.io/blog/android/2020/01/25/android-fragment-lifecycle/)에 관한 글.
[ViewLifecycleOwner](https://uchun.dev/caution-when-using-a-fragment-viewLifecycleOwner/)에 관한 글.

### LiveData

[LiveData](https://hckim999.tistory.com/26)에 관한 글.







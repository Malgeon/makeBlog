---
layout: project
author: project
title:  "Movie Clone Application"
image: assets/images/project/android/main.JPG
description: 간단한 API를 이용하여 영화 앱을 만들어보자.
categories: [ project ]
tags: [ android ]
project: true
featured: true

maindescriptions: [ "부스트코스 안드로이드 과정 중 영화 앱 프로젝트에서 주어진 API를 이용하여, 해당 API의 정보를 보여주는 영화 어플리케이션입니다.", "디자인 패턴은 MVVM, Database는 Room, viewModel data binding은 LiveData를 적용하였습니다." ]
system: [ "개발 환경: Android Studio", "Spec : kotlin, MVVVM, koin, jxJava, retrofit, Room, coroutine", "역할 : 개발" ]
developperiod : "2020.11.30 ~ "
link: "Github"
linkUrl: https://github.com/Malgeon/MovieAppPractice
imagenum: 3
images: [ assets/images/project/android/main.JPG, assets/images/project/android/content.JPG, assets/images/project/android/rating.JPG ]
---

### API 

주어진 API 상세 : 


### 앱 아키텍처

아래와 같이 안드로이드 개발자 페이지에서 추천하는 아키텍쳐로 구성하였다.

![final-architecture]({{ site.baseurl }}/assets/images/project/android/final-architecture.png)

### 디자인 아키텍쳐 : MVVM

### DI - Koin

다음 프로젝트에서는 Dagger-Hilt를 사용할 예정이다.

### NetWork - rxJava, retrofit

현재까지 이해하기로, API를 통해 Data를 받고자 할 때 비동기로 처리하기 위한 방법으로 rxJava를 사용하고 있으며 위 프로젝트에서는 API 데이터가 많지 않아 단순히 Single로 발행하고 있다. 

조금 찾아보니 Observable로 구성하여 rxBus를 만들수 있어서 확장성 측면에서 Observable로 구성하는 것이 연습하는 목적에선 더 좋을것 같긴한데(물론 이것도 현재 잘못 이해하고 있는 것일수도..), 우선 하나 프로젝트를 끝내고 싶은 마음에 시간 관계상 다음 프로젝트에서 rxJava를 본격적으로 공부를 할 예정이다.

API통신 방법은 volley보단 retrofit이 표준인 것 같다.



![android!]({{ site.baseurl }}/assets/images/project/android/android.jpg)


# 현재 작업중

### UI

- Statusbar와 Toolbar가 스크롤에 따라 투명도가 같이 변하는 함수 작업
(현재까지 방법을 찾기로는 react native나 flutter는 편하게 하는것 같은데.. kotlin은 어려워 보인다. 아직 못찾은 것일수도)

- 리스트 뷰를 동적으로 추가해주는 함수 작업



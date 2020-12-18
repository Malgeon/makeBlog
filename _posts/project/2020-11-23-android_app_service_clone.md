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

# SPEC

### API 

주어진 API 상세 : 


### 앱 아키텍처

아래와 같이 안드로이드 개발자 페이지에서 추천하는 아키텍쳐로 구성하였다.

![final-architecture]({{ site.baseurl }}/assets/images/project/android/final-architecture.png)

### 디자인 아키텍쳐 : MVVM

### DI - Koin

다음 프로젝트에서는 Dagger-Hilt를 사용할 예정이다.

### NetWork - rxJava, retrofit


![android!]({{ site.baseurl }}/assets/images/project/android/android.jpg)


# 현재 작업중

### UI

- Statusbar와 Toolbar가 스크롤에 따라 투명도가 같이 변하는 함수 작업(이미지를 화면 전체로 채우기 위함)


# 추가 작업 예정 목록

### UI

- 리스트 뷰를 동적으로 추가해주는 함수 작업


# 리팩토링

### LinearLayout -> ConstraintLayout

[ConstraintLatout](https://academy.realm.io/kr/posts/constraintlayout-it-can-do-what-now/)에 대한 글.










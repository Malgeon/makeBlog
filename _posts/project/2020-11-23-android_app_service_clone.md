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

### MVVM 디자인 패턴에 맞게 다시 구축
MVVM이 아니었다...!!!

MVVM의 ViewModel 기능에 맞도록 databinding과 viewbinding을 적용하자.

### LiveData 활용도
(MVVM의 dataBinding과 연결되어) 단순히 API의 response값만 받는 형태가 아닌, 프래그먼트 primary Key 처리 등
활용도를 높이자.

MediatorLiveData의 메커니즘을 이해하여 사용하자.

### Livecycle에 대한 이해
특정 시점에서 fragment lifecycle이 destroy되지 않는 현상에 대한 대안으로 viewLifecycleOwner를 사용하며, background에서 observing을 필요로 하는 작업에 대하여 observeForever을 사용하는 등 여러 작업을 통해 lifecycle을 이해하며 사용하도록 하자.


### UI
- Statusbar와 Toolbar가 스크롤에 따라 투명도가 같이 변하는 함수 작업(이미지를 화면 전체로 채우기 위함)

<br><br><br><br>

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







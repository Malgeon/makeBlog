---
layout: post
author: study
title:  "Android 백그라운드 처리 방법"
description: "관련 API"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/kotlin.png
---

### Service
기본적으로 UI 스레드에서 동작하기 때문에 앱의 느려짐을 방지하기 위해 별도로 처리가 필요하다.
다른 컴포넌트와 bind를 할 수 있다.

그래서 정말로 Service가 필요한 경우가(예를들어 음악 재생 앱이나 앱 스토어 같이 Service bind가 필요한 경우) 아니면 대체로 안쓰는 것이 좋다.


### JobIntentService
작업이 순차적으로 수행되며, 10분의 제한시간이 존재한다. 권한 부여가 필수

### WorkManager
대부분의 서비스로 할수 있는것을 대체 가능하며, 오래 걸리는 작업을 Worker로 구현한다.
한 번만 수행과 반복 수행 등의 동작이 가능하며, 네트워크오 배터리 상태 등으로 트리거가 가능하다.
foregroundService 로도 동작할 수 있다.


![background]({{ site.baseurl }}/assets/images/study/android/backgroundmanage.JPG)




### Immediate

- 코틀린 코루틴

- 자바 ExecutorService
(Asynk가 안되므로)

- WorkManager
약간의 Delay가 있어도 된다면 그냥 WorkManager를 사용하면 된다.


### Exact

- AlarmManager

- WorkManager


### Deferred

- WorkManager
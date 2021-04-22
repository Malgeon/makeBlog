---
layout: project
author: project
title: "Useful Photo Album"
image: assets/images/project/android/useful_project_2.JPG
description: 사진 에세이 어플리케이션
categories: [ project ]
tags: [ android ]
project: true
featured: true

maindescriptions: [ "일기, 글귀와 같은 어떤 글을 사진과 함께 쓰고 싶을 때, Useful Photo Album은 원하는 사진을 찾아 글을 쓸수 있게 해줍니다.", "오픈 API를 활용하여 사진을 제공하며 글을 저장하는 어플리케이션 입니다." ]
system: [ "개발 환경: Android Studio", "Spec : kotlin, MVVVM, ACC, Dagger-Hilt, retrofit, coroutine", "역할 : 기획, 디자인, 개발" ]
developperiod : "2021.02.25 ~ "
link: "Github"
linkUrl: https://github.com/Malgeon/useful-photo-album
imagenum: 3
images: [ assets/images/project/android/useful_project_2.JPG, assets/images/project/android/useful_project_3.JPG, assets/images/project/android/useful_project_4.JPG ]
---

# release note

### 210401
- API response에서 paging response가 아닌 random query를 paging으로 묶어 기존 search query에 적용한 adapter로 사용하게 만들었습니다. 
- onCreateView()에서 진행하던 view 관련 작업을 확실하게 View가 다 그려진 이후에 불리는 onViewCreated()로 옮겼습니다.

### 210407
- paging response가 아닌 random query에 대하여 Resource를 부여하여 loading progress UI를 적용하였습니다.


# 개발 이슈

- loading response 구현
- Random query data 유지(로컬 데이터베이스에 저장)
- 배너 및 feature page 구현을 위한 작업(DB + RecyclerView)
- Design

# 기술 SPEC

- Dagger-Hilt
- ACC(LiveData, Room, viewmodel) - ViewModel, DataBinding
- Flow - Coroutine
- paging3

### Paging
---
layout: post
author: study
title:  "Spring : Layerd Achitecture"
description: "Layerd Achitecture에 대하여"
categories: [ study ]
tags: [c, java]
---

# Spring - Layered Achitectur

## 개요
 처음 Spring Framework Module에서 볼 때, 아마도 web 개발을 MVC를 통해서. DB관리를 data Access를 통해서 독자적으로 하도록 나누고자 할때 이 둘을 체계적으로 관리할 필요에 의해 존재하는 것 같다. 그와 동시에 각 부분에서 전문화 되도록 관리 하도록 도와주는 것 같다.

## Layered Achitectur
 우선 xml에서 listener를 통해 MVC와 Data Access를 Data Access가 root, MVC는 그 밑으로 정의 한다
 - listener : 특정한 이벤트가 발생할 때 동작한다.
 
 - ContextLodaerListener : listener에 의해 Context가 Loading 될때 수행 하는데 parameter로 AnnotationConfigWebApplicationContext와 Data Access의 Main Configuration을 가지고 온다.(보통 ApplicationConfig)
 

 - filter : 요청이 수행되기 전 응답이 나가기 전 한번 씩 걸쳐나갈수 있게 해준다.

 

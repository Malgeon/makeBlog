---
layout: post
author: study
title:  "SPRING : Mvc"
description: "Mvc 동작 이해"
categories: [ Study ]
tags: [C, Java]
---

# Spring MVC

## 개요
 서블릿과 같은 웹 구현을 위한 작업물을 관리하며 도와준다.
 구체적으로 어떤 도움을 주는지 알아보자.

### WEB
 
 `spring-webmvc에서 다루고 있다.`
 - org.springframework.web.servlet.DispatcherServlet : 프론트 컨트롤러에 해당한다. 초기화함께 @EnableWebMvc 를 찾는다.
  xml에서 parameter로 AnnotationConfigWebApplicationContext와 FrontController를 도와줄 Main에 해당하는 클래스(보통 WebMvcContextConfiguration) 을 받는다.
   - WebMvcConfigurerAdapter : 초기 기본 설정 이외 필요한 동작을 할수 있게 해준다.
     - addResourceHandlers(__) : http 같은 경우 css와 js이 있는 곳을 연결해 준다. ResourceHandlerRegistry 객체를 받아 입력.
     - configureDefaultServletHandling(__) : //Mapping 정보가 없는 url을 defaultservlet이 처리하게끔 enable! -> was에서 static한 자원을 읽어서 보여준다. DefaultServletHandlerConfigurer 객체를 받아 enable 사용.
     - addViewControllers(__) : Controller annotation 하지 않아도 특정 url에 대해 Mapping 해준다.  addViewControllers에서 주소값을 설정해주고, setViewName에서 해당 view 파일을 보여준다.
     - getInternalResourceViewResolver(__) : view 파일의 위치를 정해준다. InternalResourceViewResolver 할당하여 해당 객체에 prefix로 주소를 serffix에 확장자를 더해서 반환한다.

 - ModelMap : Request scope을 직접 사용하기 보다 Spring이 제공하는 데이터 전달 객체. Spring이 알아서 Request scope로 전달해준다. (여러 객체 존재 Model, ModelView 등등)






 

 

---
layout: post
author: study
title:  "Spring : Web-Api"
description: "Web-Api 동작 이해"
categories: [ study ]
tags: [java]
---

# Web-Api

## 개요
 Api가 무엇인지에 대해서 보다 Web-Api에서 도움을 주는 라이브러리에 대해 살펴보고자 한다.
 
## javax.servlet
 Http 통신을 위한 라이브러리.
 현재 WAS Runtime을 Tomcat으로 지정했고, Tomcat에는 서블릿 라이브러리가 있기 때문에 바로 Tomcat에 올릴경우 없어도 되지만, 올리지 않을 경우 컴파일 단계에서 필요하다. 따라서 scope는 provided
 
## javax.jstl
 jstl 문법과 EL 표기법을 사용하게 도와주는 라이브러리. (EL표기법은 다이나믹 웹 모듈 2.4 이후부터 지원)
  - 웹 모듈 3.1이 되도록 하는 방법
   web.xml : 
   ``` xml 
   <!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd" >
   ```
   ↓
   ``` xml 
   <?xml version="1.0" encoding="UTF-8"?>
    ```

    .settings/org.eclipse.wst.common.project.facet.core.xml :
     version 변경.

## mysql-connector-java
 JDBC를 사용할 수 있게 해준다.
 jDBC: Java Database Connectivity
 MySQL 사이트에서 가지고 올수 있으며, 자바를 이용한 데이터베이스 접속 및 쿼리 실행 데이터 핸들링을 제공한다.
 

## jackson-databind
 json 라이브러리이다. 
 json 데이터를 사용할수 있게 해준다.
 
## javax.servlet.jsp
 jsp api를 만들수 있게 해준다.

  


 

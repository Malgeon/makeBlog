---
layout: post
author: study
title:  "MySql"
description: "MySql 관련 Command"
categories: [ study ]
tags: [c, java]
---

# MySQL

## 개요
 알아두면 좋을 MySQL을 사용해보며 잊기 전에 명령어 등을 작성해두자.

## MySQL 유저 생성 및 삭제
 root계정으로 접속하여 유저를 생성한다.

  - 생성
   create user '아이디'@'%' identified by '비밀번호';
  
  - 삭제
   drop user '아이디';
   delete from user whrer user ='아이디';

## 권한 부여
 grant all privileges on *.* to '아이디'@'%'; // 모든 데이터베이스의 모든 테이블에 대한 권한 부여(*.* 이 모든 테이블이란 뜻이다.) 

  grant all privileges on DB이름.* to '아이디'@'%'; //  특정 DB에 대한 모든 권한 부여.

  grant select, insert, update on DB이름.* to '아이디'@'%'; // 특정 DB에 대한 특정 권한(select,insert,update) 부여.

  FLUSH PRIVILEGES; // 변경한 권한을 즉시 반영시켜주는 명령어 


## SQL문 실행하기
  - 예시.sql 이 있는 폴더로 이동한 후, 아래와 같은 명령 수행. 
    명령 수행 후 계정 암호 입력.
   `mysql -u user아이디 -p 대상database < 예시.sql`
  
  - MySQL에 접속 후 해당 database에서 아래와 같은 명령문 실행
   `mysql> source C:/..(파일주소)../예시.sql`

## MySQL utf-8 적용하기

 MySQL이 설치된 폴더에 가면 my-default.ini 파일을 복사하여 my.ini 파일을 만든다.
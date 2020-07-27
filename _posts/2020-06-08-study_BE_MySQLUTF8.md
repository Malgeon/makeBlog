---
layout: post
author: study
title:  "MySqlUTF8"
description: "MySql 한글 입력 방법"
categories: [ Study ]
tags: [C, Java]
---

# MySQL

## 개요
 MySQL 윈도우작업시 한글 입력하지 못하는 현상을 혹시 몰라 작성해둔다.

## UTF-8 적용여부 확인
  show variables like 'c%';
  stauts;
  
  사실 하나씩 테이블 alter로 변경하는 방법이 있으나, 매우 귀찮기 때문에 새로 my.ini를 만들어 변경하는 방법을 사용한다.

## basedir 경로
  show variables where variable_name like '%dir';

  해당 경로에 my.ini파일을 집어 넣어야 한다.


## my.ini
 메모장을 열어 my.ini파일을 만들어 보자.
 
  

## MySQL utf-8 적용하기

 MySQL이 설치된 폴도에 가면 my-default.ini 파일을 복사하여 my.ini 파일을 만든다.
 들어갈 내용은 show variables where variable_name like '%dir';에서
  basedir
  datadir
  
  그리고 port

  port 번호는 status 또는 show global variables like 'port';로 확인 가능하다.
  이때 인코딩을 ANSI로 저장하자.
  저장한 my.ini는 basedir에 저장하자.
  

  mysql 서비스 종료 후 재시작.

  다시 show variables where variable_name like '%dir'; 를 통해
  basedir
  datadir
  port
  값이 이상이 없을 경우 
  
  ```
  [client]
  default-character-set = utf8
  
  [mysqld]
  character-set-client-handshake=FALSE
  init_connect=&quot;SET collation_connection = utf8_general_ci&quot;
  init_connect=&quot;SET NAMES utf8&quot;
  character-set-server = utf8
  collation-server = utf8_general_ci
 
  [mysqldump]
  default-character-set = utf8
 
  [mysql]
  default-character-set = utf8
  ```

  추가 후 저장
  mysql 서비스 종료 후 재시작.

  해결.
  
  
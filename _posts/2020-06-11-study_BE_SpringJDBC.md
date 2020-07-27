---
layout: post
author: study
title:  "Spring : JDBC"
description: "JDBC에 대하여"
categories: [ study ]
tags: [c, java]
---

# Spring JDBC

## 개요
 Database와 연결하는 동작을 도와준다. 
 구체적으로 어떤 도움을 주는지 알아보자.

### Data Access 
 DataBase와 연결 부분을 도와준다.

 `spring-jdbc에서 다루고 있다.`
 `spring-tx에서 다루고 있다.`

 `또한 추가로 db 연결을 위해 mysql-connector-java가 필요하다.`
 `basic data source를 다루기 위해 apache 제공하는 commons-dbcp2 사용한다.`
  javax.sql.DataSource 객체(이 객체는 다음에 공부하도록 하자.)에 apache에서 제공하는 basicDataSource를 사용하여 데이터를 저장하여 관리한다. 

 jdbcTemplate.queryForInt : 쿼리문을 작성해서 결과 데이터 수를 return
 jdbcTemplate.queryForObject : 쿼리 결과를 원하는 객체로 받을 수 있다.
 이때 new Object[]{1212L}를 사용하는 이유를 잘 모르겠다.

 - NamedparameterJdbcTemplate : jdbcTempate에서 인자를 ?로 받는 대신 파라미터명을 사용하여 작성하는 것을 지원한다. 
  NamedparameterJdbcTemplate 
  - query Method : query(Sql문, 바인딩 할 Parameter를 위한 빈 객체- collection.empty객체, dto에서 가져온 rowMapper)

   - RowMapper : Dto 클래스를 가져와 해당 필드를 parameter로 바꿔준다. (BeanpropertyRowMapper => 카멜 표기법 <-> 언더스코어 표기법) 즉, 반환을 Dto 객체로 하고 싶을 경우 해당 틀을 RowMapper가 제공하는 것 같다.

  와 신기하네 그렇다면 (NamedparameterJdbcTemplate)query(쿼리문, 빈객체, rowMapper) = Dto 객체 이렇단 소린데 자동으로 연결해준단 소리인건가?

  - update Method : 쿼리문과 바인딩할 parameter 객체를 받아 datebase값을 변경한다. (UPDATE, DELETE) 성공시 1, 실패시 0 반환
   - UPDATE시 SqlparameterSource에 BeanPropertSqlParameterSource에서 Dto를 받아 각 parameter를 전달한다. 전달된 parameter를 Query문에 바인딩 하여 실행.
   - DELETE시 Map에 parameter와 값을 가진 singletonMap(1개를 지우는데 사용!)으로 parameter 전달한다. 전달된 parameter를 Query문에 바인딩 하여 실행.
  
  - queryForObject Method : queryForObject(SQL 문 , Map에 parameter와 값을 가진 singletonMap으로 parameter 전달한다. , Dto 객체로 반환하기 위해 RowMapper) 전달된 parameter를 Query문에 바인딩 하여 실행. 이떄 해당 parameter에 값이 없을 경우를 대비(Exception) 


 - SimpleJdbcInsert : insert 쿼리를 생성해준다. dataSource에 객체를 생성할때 whtiTableName에 table명을 넣어주면 생성 되며, 객체 안 execute를 통해 쿼리를 실행한다. usingGeneratedKeyColumns("파라미터")을 통해 자동으로 파라미터의 값을 가진다.(보통 id) 그리고 excuteAndReturnKey를 통해 반환한다.

 - TransactionManagementConfigurer : 보통 transaction 하기 위해 DBConfig에서 implements 받아 사용한다. 
   - annotationDrivenTrasactionManager() : transaction을 하기 위해 PlatformTransactionManager의 transactionManager()를 사용하여 DataSourceTransactionManager를 DataSource로 할당시키며, PlatformTransactionManager을 반환한다.




 

 

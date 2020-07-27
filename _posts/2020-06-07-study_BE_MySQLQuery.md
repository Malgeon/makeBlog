---
layout: post
author: study
title:  "MySQL Query"
description: "Database Query문에 대해서 알아보자."
categories: [ study ]
tags: [c, java]
---

# MySQL

## 개요
 Database Query문에 대해 알아보자.

## Table Join
 기본 명렁어는 ~ FROM 기준테이블 JOIN join할테이블 ON 조건
 
### Cross Join
 Table A의 모든 레코드들에 대해 Table B가 모두 Mapping 되는 것.
  Table A 레코드 10개 Table B 레코드 5개 = 10 X 5 = 50개 출력
 단순하 From절에 복수의 테이블 지정
  
   SELECT * FROM table_a, table_b


### Inner Join
 Table A의 특정 컬럼값이 Table B의 지정 컬럼값과 일치하는 레코드만을 선별.

  ~ FROM table_a INNER JOIN table_b ON table_a.Col = table_b.Col

  ~ FROM table_a, table_b WHERE table_a.Col = table_b.Col

  SELECT table_a.ID, table_b.* FROM table_a INNER JOIN table_b ON table_a.ID = table_b.ID WHERE Table_a.Date >= '1/1/2012'

 Alias를 사용하여 간략히
  SELECT a.ID, b.* FROM table_a a INNER JOIN table_b b ON a.ID = b.ID WHERE a.Date >= '1/1/2012'

### Outer Join
 Inner Join + 매칭이 되지 않는 나머지 레코드 또한 출력(NULL) (원하는 레코드에 따라 left, right, full로 나뉜다.)
  
  SELECT a.Name, SUM(b.Total) '합계' FROM table_a a LEFT OUTER JOIN table_b b ON a.Code = b.CityCode GROUP BY a.Name

 ### Self Join
 자신을 Join함
  
  SELECT a.EmpID, a.Name, b.Name AS Boss FROM table_a a, table_b b  WHERE a.MgrId = b.EmpId AND b.Name = 'Kim'

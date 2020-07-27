---
layout: post
author: study
title:  "CORS와 JSONP"
description: "도메인간 Ajax 통신을 위한 방법"
categories: [ Study ]
tags: [C, Java]
---

# JSON

 연습 프로젝트 작업 중 다음과 같은 경고문을 보았다.

  `Access to XMLHttpRequest at 'http://#' from origin 'http://@' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

 CORS 정책에 block 되었다는 건데, #의 도메인에서 XMLHttpRequest로 @도메인에 접근을 하게되면 보안상의 이유로 접근을 못하게 된다.

 Rest한 API을 만드는 작업 중 url에서 Json 파싱을 가능하게 하는 작업이 있다. 이때 해당 url은 database 객체 자료이기 때문에, 누구나 url을 요청하게 되면 해당 database를 public으로 이용하는 것이나 다름없게 된다. 이 같은 이유가 아마도 보안상의 이유라고 생각한다.

 내 경우는 파일 Http에서 작업하면서 XMLHttpRequest를 로컬 서버에 요청을 하다 보니 block 된 것이다.

 물론 로컬 서버에서 작업을 해도 되지만, 여러가지 이유로 도메인간 접근을 가능하게 해야할 경우가 많다. 때문에 당연히 알아야 할 내용이므로 공부하기로 한다.

 그렇다면 도메인간 XMLHttpRequest를 사용하려면 어떻게 해야 할까.
  
  // 공부하다보니 너무 어려워서 일단 보류

# CORS
 
 경고문에서 설명하 듯 block 하는 CORS 정책을 해제해주는 방법이 존재한다.

 우선, 단순히 서버에서 CORS 정책을 해제하는 것으로 이해하고, 프레임워크에서 도와주는 것이 존재한다. 정도로 넘어가자.

 ## @CrossOrigin
  Spring에서는 @RequestMapping 위에 @CrossOrigin(origins = "도메인 주소, 도메인 주소 ... ") 로 사용하여 도와준다.

# JSONP
  json 값을 js를 통해 주고 받게 만드는 방법이다. 우회의 일종이랄까.
 
 


 
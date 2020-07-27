---
layout: post
author: project
title:  "Spring : Container"
description: "Spring의 동작원리를 이해해보자"
categories: [ Project ]
tags: [ Web ]
image: assets/images/project/java_spring.jpg
featured: true
hidden: true
project: true
---

# Spring - Container

## 개요

## Core Container

### Been
 프로젝트시 대표적인 반복작업인 new 메소드를 통한 메모리 할당을 Spring에서는 해당 작업을 Bean을 통해 도와준다.

 `그리고 Bean을 spring-context에서 다루고 있다.`

 아마도 메모리 할당을 위한 클래스를 만들 때 다음과 같은 약속은 당연한것 같다.
 - 자유도가 넓어야 한다. 즉, 기본 생성자를 가지고 있어야 한다.
 - 보안 측면에서 필드는 private로 선언.
 - 필드가 private이기 떄문에 getter, setter 메소드는 필수.

 그렇기 때문에 위와 같은 약속이 Bean의 조건인건 자연스럽다. 위와 같은 조건을 만족한 Class를 Bean Class로 사용할수 있다. 
 
 추가로 Bean에서 아마도 확장성의 이유로 각각의 getter, setter를 프로퍼티라고 약속하고 관리한다. (ex setName => name이란 name을 가진 프로퍼티)

 Bean Class로 사용할수 있는 Class를 사용하기 위해선 '사용하겠다!'라고 선언(Bean 선언)을 해야한다. 
 그리고 Spring은 Bean 선언을 확인하고 getBean()을 통해 메모리 할당을 한다. 
 할당은 SingleTon. 즉, 여러번 getBean()을 하더라도 결국 인스턴스는 하나인 방식이다.
 
 선언하는 방법은 여러 방법이 있다.

 - Xml에 선언
 Bean 선언을 Xml에 선언하여 따로 관리한다. 보통 src/main/resources에 applicationContext.xml로 관리한다. 이때, Spring:ApplicationContext가 Spring:ClassPathXmlApplicationContext 에서 'xml파일 이름'을 받아 src파일 내에 받은 xml 파일 이름에 xml 파일을 찾아 작성된 선언을 확인한다.
 프로퍼티를 활용하여 Bean Class를 객체로 가진 Bean Class를 다룰수도 있다.

 - Class에 선언 (annotation 이용)
 Class에 선언하여 따로 관리한다. '@annotation'과 같이 Spring에서는 단순 Bean 선언과 더불어 더 많은 annotation을 지원하여 각 annotation에 맞는 다양한 동작을 도와준다. 
 
   - @configuration : Xml을 Class로 가져온 것이라고 할수 있는데, 선언문을 가진 class임을 나타낸다. 보통 configuration이기 때문에 config라는 패키지에 각 목적에 맞는 Config.class로 관리한다. Main config는 ApplicationConfig이때, Spring:ApplicationContext가 Spring:AnnotationConfigApplicationContext 에서 (이름.class) 또는 ("이름") 을 받아 해당 클래스에 annotation이 있는지 확인하고 선언을 다룬다. 클래스를 가지고 오기 때문에 method이름이 달라서 생기는 error를 걱정할 필요가 없다.

   - @Bean : Bean Class를 메모리 할당을 해주는 method임을 나타낸다. @configuration클레스에서 실제 메모리 할당하는 작업을 작성한다.

   - @Import : 두개 이상의 Configuration을 나눠서 다루고자 할때 사용한다. import할 클래스 이름을 받아 해당 클래스의 Configuration을 가져다 쓴다.

   - @ComponenetScan : annotation 선헌 할때 root를 받아[(프로젝트 전체), 패키지를 받으려면 (basePackages - {"패키지"})] root에 속해있는 Class 중 ComponenetScan이 Scan할수 있는 annotation을 찾고 찾은 Class에서 각 annotation에 맞는 동작을 한다. (@Component, @Repository, @Controller, @Service)
     - @Component : Bean Class로 사용안 Class안에 있으며 Bean Class임을 나타낸다.
       - @Autowired : Bean Class에서 다른 Bean 객체를 받게 될 경우 자동으로 해주도록 나타낸다.
     - @Repository : 저장소의 역할을 나타낸다.

   - @EnableTransactionManagement : transaction과 관련된 설정을 자동으로 해준다. 단 TransactionManagementConfigurer를 implements 받아 구현해야 하며, annotationDrivenTransactionManager()를 오버라이딩 해야 한다.
    `그리고 spring-jdbc에서 다루고 있다. 왜 그런지는 모르겟는데, 사실 transaction은 spring-tx에서 관리할것 처럼 이지만 jdbc에서 관리하고 있다. 여하튼 jdbc를 사용하려면 tx와 같이 사용하고 있다.`
     - @Primary


### MVC
 웹에서 서블릿 반복 작업을 Spring에서는 WebMvc을 통해 도와준다.

 `그리고 WebMvc는 spring-webmvc에서 다루고 있다.`

 - @EnableWebMvc

 생각해 보자. 
 웹에서 어떤 주소를 입력을 하면 Http에서 해당 주소에 Request를 보낸다. 그렇다면 그 주소에 해당하는 곳에서 2가지 방법으로 받을수 있는데, xml방식과 자바 class(annotation) 방법이다.

   - xml
   xml에서 해당주소를
    주소 - sevlet name : sevlet name - sevlet class 로 매핑을 시킨다.
    그러면 해당 sevlet 클래스가 나오는 것이다.
  
   - class
   서블릿 경우 @WebServlet
   컨트롤러의 경우도 있으나 이 경우는 Xml을 사용하지 않고 쓸 장점이 별로 없어서 xml을 사용한다.

 여하튼 두가지 방법으로 받아오면 servlet class로 org.springframework.web.servlet.DispatcherServlet를 연결시키고 Main Controller를 parameter로 가지고 오는데(보통 WebMvcContextConfiguration) 해당 클래스에서  @EnableWebMvc을 선언하면 초기화 해준다.

 - @ComponenetScan : 앞서 설명함.
     - @Controller : 서블릿으로 사용할 Class를 나타낸다.
         - @RequestMapping : Http 요창과 이를 다루기 위한 Controller의 메소드를 연결한다. 
           - @GetMapping : @RequestMapping(path="", method=RequestMethod.Get)
           - @PostMapping : @RequestMapping(path="", method=RequestMethod.Post)
           - @PutMapping
           - @DeleteMapping
           - @PatchMapping
         
         - @RequestParam : Mapping된 Method의 Argument에 붙일수 있게한다. required는 필수 여부 판단.
            - defaultValue – This is the default value as a fallback mechanism if request is not having the value or it is empty.
            - name – Name of the parameter to bind
            - required – Whether the parameter is mandatory or not. If it is true, failing to send that parameter will fail. 
            - value – This is an alias for the name attribute

         - @PathVariable : 상위 Mapping의 path에 변수명을 받아와 사용할 수 있게 한다.
         - @RequestHeader : 요청정보의 헤더 받아온다.
         - @ModelAttribute : Dto 객체 앞에 붙여서 해당 Dto를 parameter로 사용할 수 있게 해준다.
      

 - @Service : 서비스 Class임을 나타낸다.
 - @Transactional : Default는 ReadOnly이다. 해당 ReadOnly를 false를 해주게 되면 동시성으로 안될 떄 rollback해주게 된다.

 - @RestController : Controller를 Rest API로 만들게 해준다. 하나의 url에 각각의 작업을 해준다.
 

 


 


 


  


 

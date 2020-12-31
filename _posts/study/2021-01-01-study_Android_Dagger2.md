---
layout: post
author: study
title:  "Android DI Dagger2"
description: "Dagger2를 적용해 보자"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---

[이전 DI 적용](../study_Android_DI)

### Dagger2

지난 포스트에서 다뤘던 CoffeeMaker에서 더욱 발전시켜 CoffeeMaker와 CafeInfo로 구성된 CafeApp을 만들어 보고자 한다.

#### Setting

##### CoffeeMaker

편의상 이전에 만든 CoffeeMaker에서 Pumb는 제외하는 대신 brew에 CoffeeBean을 받도록 한다.

```java
public class CoffeeMaker{
    private Heater heater;

    @Inject
    public CoffeeMaker(Heater heater){
        this.heater = heater;
    }

    public void brew(CoffeeBean coffeeBean){
        System.out.println("CoffeeBeen("+coffeeBean.toString()+") [_]P coffee! [_]P ");
    }
}
```

##### CafeInfo

단순히 카페 이름을 가지고 있는 클래스이다.

```java
public class CafeInfo {

    private String name;

    public CafeInfo(){}

    public CafeInfo(String name){ this.name = name; }

    public void welcome(){
        System.out.println("Welcome " + name == null? "":name );
    }
}
```
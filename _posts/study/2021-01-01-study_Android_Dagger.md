---
layout: post
author: study
title:  "Android Dagger"
description: "Dagger를 적용해 보자"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---

[이전 DI 적용](../study_Android_DI)

### Dagger

지난 포스트에서 다뤘던 CoffeeMaker에서 더욱 발전시켜 CoffeeMaker와 CafeInfo로 구성된 CafeApp을 만들어 보고자 한다. (CafeInfo는 단순히 카페 정보를 다루는 클래스이다.)

그리고 Dagger를 적용하는 이유에 중점을 두었던 지난 포스트와는 다르게 Dagger가 동작하는 원리를 이해보고자 한다. 

또한 프로그램을 발전시킴에 따라 필요해진 기능을 적용해보고자 한다.
<br><br>

#### 이전 Setting과 동일하게

##### CoffeeBean

CoffeeMaker의 brew 기능을 추가하여 CoffeeBean을 받도록 하겠다.

```java
public class CoffeeBean {
    public void name(){
        System.out.println("CoffeeBean");
    }
}
```

##### CoffeeMaker

편의상 이전에 만든 CoffeeMaker에서 brew에 CoffeeBean을 받도록 하는 대신 Pumb는 제외하겠다.

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

##### CafeModule

```java
@Module
public class CafeModule {
    @Provides
    CafeInfo provideCafeInfo(){
        return new CafeInfo();
    }

    @Provides
    CoffeeMaker provideCoffeeMaker(Heater heater){
        return new CoffeeMaker(heater);
    }

    @Provides
    Heater provideHeater(){
        return new Heater();
    }
}
```

##### CafeComponent

```java
@Component(modules = {
        CafeModule.class
})
public interface CafeComponent {
    CafeInfo cafeInfo();
    CoffeeMaker coffeeMaker();
}
```

### 공부 중 ... 아 진짜 러닝커브 높다.. 
.. 그러니까 이제 프로젝트를 빌드하면 Dagger는 자동으로 프로젝트 내의 Inject 항목을 구현하는 Factory를 만든다는거지? 그리고 Component의 interface를 구현하는 Dagger__Component Class를 만들고 종속 항목 그래프를 구성해 놓는다는 거지??




[위 글은 이 포스트에서 참고하였습니다.](https://cmcmcmcm.blog/2017/08/02/didependency-injection-%ec%99%80-dagger2-2/)



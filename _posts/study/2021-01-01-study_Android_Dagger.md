---
layout: post
author: study
title:  "Android Dagger"
description: "Dagger를 더 많이 알아보자."
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---

[이전 DI 적용](../study_Android_DI)

### Dagger

지난 포스트에서 다뤘던 CoffeeMaker에서 더욱 발전시켜 CoffeeMaker와 CafeInfo로 구성된 CafeApp을 만들어 보고자 한다. (CafeInfo는 단순히 카페 정보를 다루는 클래스이다.)
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

굳이 2가지 방법을 사용할 필요가 없으므로 여기선 provision method 만을 사용한다.

```java
@Component(modules = {
        CafeModule.class
})
public interface CafeComponent {
    CafeInfo cafeInfo();
    CoffeeMaker coffeeMaker();
}
```

##### Main

```java
public static void main(String[] args) {
    CafeComponent cafeComponent = DaggerCafeComponent.create()
    CafeInfo cafeInfo = cafeComponent.cafeInfo();
}
```

```java
public static void main(String[] args) {
    CafeComponent cafeComponent = DaggerCafeComponent.create()
    CafeInfo cafeInfo1 = cafeComponent.cafeInfo();
    CafeInfo cafeInfo2 = cafeComponent.cafeInfo();
    System.out.println("CafeInfo is equal : " + cafeInfo1.equals(cafeInfo2));
}
```

```
CafeInfo is equal : false
```

그런데 프로그램에서 여러가지 이유로 고유한 인스턴스를 가지고 있어야 할 필요가 있다.
이 프로그램에선 cafeInfo가 그런 예로, 단순히 비유를 하자면 카페가 운영되는 동안 카페 정보는 하나 뿐이며 누가 보더라도 같은 결과를 가져야 한다. 즉, 고유해야한다.


이를 해결 위한 방법은 @Singleton 이다.

#### @Singleton

Singleton은 고유한 인스턴스를 갖도록 해주는 annotation이다.

우선 당연하게도 cafeComponent.cafeInfo()가 singleton이 되어야 하려면 cafeComponent가 singleton이어야 하며

```java
@Singleton
@Component(modules = {
        CafeModule.class
})
public interface CafeComponent {
    CafeInfo cafeInfo();
}
```

cafeComponent.cafeInfo()가 singleton 이어야 하는데, 이것은 Module에서 provide 해주는 것이다.

```java
@Module
public class CafeModule {
    @Singleton
    @Provides
    CafeInfo provideCafeInfo(){
        return new CafeInfo();
    }
}
```

그렇다면 동일하게 된다.

```java
public static void main(String[] args) {
    CafeComponent cafeComponent = DaggerCafeComponent.create()
    CafeInfo cafeInfo1 = cafeComponent.cafeInfo();
    CafeInfo cafeInfo2 = cafeComponent.cafeInfo();
    System.out.println("CafeInfo is equal : " + cafeInfo1.equals(cafeInfo2));
}
```

```
CafeInfo is equal : true
```


여기서 추가로 CafeComponent에서 coffeeMaker는 고유한 인스턴스가 아니어도 되지만, Heater는 CoffeeMaker에서(!) 고유한 인터페이스로 만들 수도 있다.








### 공부 중 ... 아 진짜 러닝커브 높다.. 
이제 SubComponent와 CustomScope 그리고 Binds 정도가 남은건가...?



[위 글은 이 포스트에서 참고하였습니다.](https://cmcmcmcm.blog/2017/08/02/didependency-injection-%ec%99%80-dagger2-2/)



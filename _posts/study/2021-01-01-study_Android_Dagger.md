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

#### SubComponent

여기서 추가로 CafeComponent에서 coffeeMaker는 고유한 인스턴스가 아니어도 되지만, Heater는 CoffeeMaker에서(!) 고유한 인터페이스로 만들고 싶다.
그동안 공부한 Dagger를 살펴보면 CafeComponent라는 기준점으로 Singleton으로 관리할 수 있었다. 따라서 추가 기준점을 만들어야 한다. 
그러기 위한 방법이 SubComponent이다.

적용해 보자.

##### 수정

Component에서 subcompuonent를 구현하고, subcomponent는 Component와 연결된 module에서 설정한다.(encapsulate)
이는 하나의 컴포넌트에서 서브컴포넌트를 다루도록 하게 해준다.

```java
@Singleton
@Component(modules = {
        CafeModule.class
})
public interface CafeComponent {
    CafeInfo cafeInfo();
    // coffeeMaker는 subcomponent에 의해 다뤄지므로 대체한다.
    //Builder는 subcomponent에서 설명하겠다.
    CoffeeComponent.Builder coffeeComponent(); 
}
```

```java
@Module(subcomponents = CoffeeComponent.class)
public class CafeModule {
    @Singleton
    @Provides
    CafeInfo cafeInfo(){
        return new CafeInfo();
    }
}
```

##### 추가

```java
@Scope
@Retention(RetentionPolicy.RUNTIME)
public @interface CoffeeScope {}
```

```java
public class CoffeeBean {
    public void name(){
        System.out.println("CoffeeBean");
    }
}
```

아래는 subcomponent.
builder는 dagger의 component의 인터페이스를 구현하는 빌드 작업에 해당하는 인터페이스이다. @Component에서 별다른 작업이 없었던 것은 생략해도 위와 같은 동작을 자동으로 해주기 때문이다. 그러나 subcomponent에서는 생략할수가 없다.

또한 builder를 작성하면 build 하기 전에 모듈을 파라미터로 넣을수가 있는 장점이 존재한다.

```java
@CoffeeScope
@Subcomponent(modules = {
        CoffeeModule.class
})
public interface CoffeeComponent {
    CoffeeMaker coffeeMaker();
    CoffeeBean coffeeBean();

    @Subcomponent.Builder
    interface Builder{
        CoffeeComponent build();
    }
}
```

```java
@Module
public class CoffeeModule {

    @CoffeeScope
    @Provides
    CoffeeMaker provideCoffeeMaker(Heater heater){
        return new CoffeeMaker(heater);
    }

    @CoffeeScope
    @Provides
    Heater provideHeater(){
        return new Heater();
    }

    @Provides
    CoffeeBean provideCoffeeBean(){
        return new CoffeeBean();
    }
}
```

##### Main

```java
public static void main(String[] args){
    CafeComponent cafeComponent = DaggerCafeComponent.create();

    CoffeeComponent coffeeComponent1 = cafeComponent.coffeeComponent().build();
    CoffeeComponent coffeeComponent2 = cafeComponent.coffeeComponent().build();
    CoffeeMaker coffeeMaker1 = coffeeComponent1.coffeeMaker();
    CoffeeMaker coffeeMaker2 = coffeeComponent1.coffeeMaker();
    System.out.println("CoffeeScope / same component coffeeMaker is equal : " + coffeeMaker1.equals(coffeeMaker2));
    CoffeeMaker coffeeMaker3 = coffeeComponent2.coffeeMaker(); //MakerScopeMethod
    System.out.println("CoffeeScope / different component coffeeMaker is equal : " + coffeeMaker1.equals(coffeeMaker3));

    //Non-scope
    CoffeeBean coffeeBean1 = coffeeComponent1.coffeeBean();
    CoffeeBean coffeeBean2 = coffeeComponent1.coffeeBean();
    System.out.println("Non-scoped coffeebean is equal : " + coffeeBean1.equals(coffeeBean2));

    //Encapsulate
        CoffeeComponent coffeeComponent = DaggerCafeComponent.create().coffeeComponent().build();
        coffeeComponent.coffeeMaker().brew(coffeeComponent.coffeeBean());
}
```

```
CoffeeScope / same component coffeeMaker is equal : true
CoffeeScope / different component coffeeMaker is equal : false
Non-scoped coffeebean is equal : false
CoffeeBeen(dagger2example.CoffeeBean@4aa298b7) [_]P coffee! [_]P 
```


#### component.builder

앞서 subcomponent에서는 builder가 필수라고 했었다. component에서는 생략을 해도 되나, 필요시 만들 수 있다.

builder를 만들 경우 동작하기 위해 create()가 아닌 build()를 사용하며, build하기 전에 파라미터를 사용할 수 있다.


여기선 모듈에 멤버변수를 만들고 이를 사용하기 위해 builder를 사용해 보겠다. 

모듈의 멤버변수를 사용하기 위해선 member-injection method을 사용할 수 없다. 당연하게도 컴포넌트 안에서 모듈을 가지고 있는 provision method에 반해 member-injection method는 모듈을 가지고 있지 않기 때문이다.

```java
@Module(subcomponents = CoffeeComponent.class)
public class CafeModule {

    private String name;

    public CafeModule(){ }

    public CafeModule(String name){
        this.name = name;
    }

    @Singleton
    @Provides
    CafeInfo cafeInfo(){
        if(name == null || name.isEmpty()) return new CafeInfo();
        else return new CafeInfo(name);
    }
}
```

```java
@Singleton
@Component(modules = {
        CafeModule.class
})
public interface CafeComponent {

    CafeInfo cafeInfo();
    CoffeeComponent.Builder coffeeComponent();

    @Component.Builder
    interface Builder{
        Builder cafeModule(CafeModule cafeModule);
        CafeComponent build();
    }

    void inject(CafeInfo cafeInfo);
}
```

```java
public static void main(String[] args){
    CafeInfo cafeInfo = new CafeInfo();

    CafeComponent cafeComponent = DaggerCafeComponent.builder()
            .cafeModule(new CafeModule("example cafe"))
            .build();
    cafeComponent.inject(cafeInfo);
    cafeInfo.welcome();

    CafeComponent cafeComponent2 = DaggerCafeComponent.builder()
            .cafeModule(new CafeModule("example cafe"))
            .build();
    cafeComponent2.cafeInfo().welcome();
}
```

```
null
example cafe
```


####  MultiBinding - @Binds

Module에서 provide할때 같은 타입이지만 다른 객체를 반환하는 경우가 있다.
(이 프로그램에서는 CoffeeBean에 에디오피아콩과, 과테말라콩 등등 을 넣어 볼 예정이다.)

이럴때 @Binds를 사용한다. 물론 @Provides와 함께 @Named 으로 관리를 할 수 있다. 그러나 [Binds보다 비효율적이다.](https://medium.com/mobile-app-development-publication/dagger-2-binds-vs-provides-cbf3c10511b5)라고 이해하면 된다.

이런 장점에도 굳이 @Named가 있는 이유는 @Binds를 사용하려면 아래와 같은 제약조건이 따르기 때문이다.

- 오직 1개의 파라미터를 가지고 있어야 한다.
- abstract 함수로 호출해야 한다.
- 해당 항목은 Injectable해야 한다.
- 해당 항목이 provide되어도 할수는 있다.


그래서 Binds를 적용해 보고자 한다. [(@Named는 여기서 참고하자.)](https://black-jin0427.tistory.com/244)


##### 우선 Injectable항목!

bind를 하기위한 모듈이 필요하며 이것도 abstract 여야 한다. 또한 같은 타입이지만 다른 객체인 항목을 구분해주기 위해 Map을 이용해 주며, Key값을 설정해야 한다. 여기선 @StringKey을 이용해 String으로 구분해 주려고 한다. (@MapKey로 커스텀 설정이 가능하다.)

```java
@Module
public abstract class CoffeeBeanModule {
    @Binds
    @IntoMap
    @StringKey("guatemala")
    abstract CoffeeBean provideGuatemalaBean(GuatemalaBean guatemalaBean);
}
```

이 모듈은 주입을 하는 component 즉, CoffeeComponent와 연결된다.

```java
@CoffeeScope
@Subcomponent(modules = {
        CoffeeModule.class
        ,CoffeeBeanModule.class
})
public interface CoffeeComponent {
    CoffeeMaker coffeeMaker();
    Map<String,CoffeeBean> coffeeBeanMap();

    @Subcomponent.Builder
    interface Builder{
        //Builder coffeeModule(CoffeeModule coffeeModule);
        CoffeeComponent build();
    }
}
```

이제 이용해 보자!

```java
public static void main(String[] args){
    CoffeeComponent coffeeComponent = DaggerCafeComponent.create().coffeeComponent().build();
    coffeeComponent.coffeeBeanMap().get("guatemala").name();
}
```

```
GuatemalaBean
```

##### provide된 항목

provide된 항목이라고 간단하게 말했지만, 풀어보자면 다음과 같다.

Component 인터페이스에 CoffeeBean을 @binds로 구성된 abstract 함수로 provide해준다.
이는 다른 모듈에서 provide된 항목으로 주입 할수 있다.
이 항목은 다시 mapkey를 이용한 항목에도 들어갈 수 있다.

```java
@CoffeeScope
@Subcomponent(modules = {
        CoffeeModule.class
        ,CoffeeBeanModule.class
})
public interface CoffeeComponent {
    CoffeeMaker coffeeMaker();
    CoffeeBean coffeeBean();

    @Subcomponent.Builder
    interface Builder{
        //Builder coffeeModule(CoffeeModule coffeeModule);
        CoffeeComponent build();
    }
}
```

```java
@Module
public abstract class CoffeeBeanModule {
    @Binds
    abstract CoffeeBean provideCoffeeBean(EthiopiaBean ethiopiaBean);

    @Binds
    @IntoMap
    @StringKey("ethiopia")
    abstract CoffeeBean provideEthiopiaBean(EthiopiaBean ethiopiaBean);
}
```

```java
@Module
public class CoffeeModule {

    @CoffeeScope
    @Provides
    CoffeeMaker provideCoffeeMaker(Heater heater){
        return new CoffeeMaker(heater);
    }

    @CoffeeScope
    @Provides
    Heater provideHeater(){
        return new Heater();
    }

    @Provides
    EthiopiaBean provideEthiopiaBean(){
        return new EthiopiaBean();
    }
}
```

```java
public static void main(String[] args){
    CoffeeComponent coffeeComponent = DaggerCafeComponent.create().coffeeComponent().build();
    coffeeComponent.coffeeBean().name();
    coffeeComponent.coffeeBeanMap().get("ethiopia").name();
}
```

```
EthiopiaBean
EthiopiaBean
```

방법이 있다 정도로 이해해 두면 좋을 것 같다.


### 결론

이 정도면 dagger를 이해하고 다루기엔 부족함이 없을 것이다.



[위 글은 이 포스트에서 참고하였습니다.](https://cmcmcmcm.blog/2017/08/02/didependency-injection-%ec%99%80-dagger2-2/)



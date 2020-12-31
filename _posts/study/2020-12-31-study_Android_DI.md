---
layout: post
author: study
title:  "Android DI Dagger"
description: "그래서 DI가 뭔데? Dagger를 쓰면 어떻게 되는데?"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---


### DI

&nbsp;[Android Devloper에서 설명하는 DI](https://developer.android.com/training/dependency-injection?hl=ko)에서 설명이 잘 되어 있지만, 해당 예시에 Dagger를 적용해서 보여주진 않는다.

&nbsp;그래서 여기서는 커피를 내리는 동작을 하는 예시 프로그램을 수동 종속 삽입(Without DI), 간단한 Injection을 작성하여 자동 종속 삽입(With DI), 그리고 Dagger를 이용하여 종속 삽입(With Dagger) 경우를 살펴보면서 DI의 필요성을, 그리고 편이성을 이해해 보고자 한다. 

#### 기본 동작

우선 Coffee를 내리도록 Maker를 셋팅하고, 동작을 구현해 보자.

```java
public class CoffeeMaker {
    private final Heater heater;
    private final Pump pump;

    public CoffeeMaker(Heater heater, Pump pump){
        this.heater = heater;
        this.pump = pump;
    }

    public void brew(){
        heater.on();
        pump.pump();
        System.out.println(" [_]P coffee! [_]P ");
        heater.off();
    }
}
```

해당 Maker에는 Heater와 Pump가 필요하다. 이때 이 둘은 어떤 종류가 오더라도 상관이 없다. 

```java
// Heater
public interface Heater {
    void on();
    void off();
    boolean isHot();
}
```

```java
// Pump
public interface Pump {
    void pump();
}
```

그리고 A 상품을 사용하려고 한다.

```java
// A Heater
public class A_Heater implements Heater {
    boolean heating;

    public A_Heater(){ }

    public void on() {
        System.out.println("A_Heater : ~ ~ ~ heating ~ ~ ~");
        this.heating = true;
    }

    public void off() {
        this.heating = false;
    }

    public boolean isHot() {
        return heating;
    }
}
```

```java
// A Pump
public class A_Pump implements Pump {
    private final Heater heater;

    public A_Pump(Heater heater) {
        this.heater = heater;
    }

    public void pump() {
        if (heater.isHot()) {
            System.out.println("A_Pump => => pumping => =>");
        }
    }
}
```

이제 동작을 시켜보자.

- 우선 직접 종속 항목을 삽입해 보자. : Without DI

```java
public static void main(String[] args) {
    Heater heater = new A_Heater(); // A_heater 를 사용해야한다는 것을 알아야 한다.
    Pump pump = new A_Pump(heater); // A_pump 를 사용해야 한다는 것을 알아야 한다.
    CoffeeMaker coffeeMaker = new CoffeeMaker(heater,pump);
    coffeeMaker.brew();
}
```

Main Phase에서 우리는 Heter를 A Heater로 Pump를 A Pump셋팅을 해줘야하고, 커피를 셋팅해 주는 다음 커피를 내려주는 것을 알 수 있다.


- 간단한 Injection을 만들어 자동 종속 항목 삽입을 해보자. : With DI

Injection를 만들자.

```java
public class Injection {
    public static Heater provideHeater(){
        return new A_Heater();
    }

    public static Pump providePump(){
        return new A_Pump(provideHeater());
    }

    public static CoffeeMaker provideCoffeeMaker(){
        return new CoffeeMaker(provideHeater(),providePump());
    }

    public static Pump providePump(Heater heater){
        return new A_Pump(heater);
    }
}
```

만든 Injection을 이용하여 커피를 내려보자.

```java
//어떤 heater와 pump가 필요한지 몰라도 된다. Injection class에 모두 정의 되어 있다.
public static void main(String[] args) {
    Heater heater = Injection.provideHeater(); // A heater 가 필요한지 B heater 가 필요한지 몰라도 된다.
    Pump pump = Injection.providePump(heater); // A pump가 필요한지 B pump가 필요한지 몰라도 된다.
    
    CoffeeMaker coffeeMaker = new CoffeeMaker(Injection.provideHeater(),Injection.providePump());
    coffeeMaker.brew();

    // 또는 coffeemaker 자체를 injection class 에서 di를 해줄수도 있다.
    Injection.provideCoffeeMaker().brew();
}
```

Main Phase에서 우리는 Heter와 Pump가 A인지, B인지 알 필요가 없다. 그것은 Injection에서 알아서 해준다. 


- 그렇다면 Dagger를 사용해보자.

[Android Devloper에서 설명하는 Dagger](https://developer.android.com/training/dependency-injection/dagger-basics?hl=ko)에서 Dagger에 필요한 annotation을 이해하도록 하자.


Module 클래스를 만들어 의존성 관계를 설정해주자.

```java
@Module
public class CoffeeMakerModule {
    @Provides
    Heater provideHeater(){
        return new A_Heater();
    }

    @Provides
    Pump providePump(Heater heater){
        return new A_Pump(heater);
    }
}
```

그리고 해당 Module을 통해 직접 주입해주는 Component 클래스를 만들자.

이때 Component는 두가지 방법으로 주입을 해줄수 있는데 

```java
@Component(modules = CoffeeMakerModule.class)
public interface CoffeeComponent {

    //provision method 
    CoffeeMaker make();

    //member-injection method
    void inject(CoffeeMaker coffeeMaker);
}
```


이제 주입을 해줘야 하는 항목들을 찾아 @Inject 해주도록 하자.

```java
// CoffeeMaker
public class CoffeeMaker {
    @Inject Heater heater;
    @Inject Pump pump;

    public CoffeeMaker(){}

    @Inject
    public CoffeeMaker(Heater heater, Pump pump){
        this.heater = heater;
        this.pump = pump;
    }

    public void brew(){
        heater.on();
        pump.pump();
        System.out.println(" [_]P coffee! [_]P ");
        heater.off();
    }

    public Heater getHeater() {
        return heater;
    }

    public Pump getPump() {
        return pump;
    }
}
```

```java
// Pump
public class A_Pump implements Pump {
    private final Heater heater;

    @Inject
    public A_Pump(Heater heater) {
        this.heater = heater;
    }

    public Heater getHeater() {
        return heater;
    }

    public void pump() {
        if (heater.isHot()) {
            System.out.println("A_Pump => => pumping => =>");
        }
    }
}
```

이제 모든 준비를 마쳤으니 실행하도록 하자.

```java
public static void main(String[] args) {
    //provision method
    DaggerCoffeeComponent.create().make().brew();

    //member-injection method
    CoffeeMaker coffeeMaker = new CoffeeMaker();
    DaggerCoffeeComponent.create().inject(coffeeMaker);
    coffeeMaker.brew();
}
```


간단한 Dagger 사용이다.

다음은 더욱 발전시켜 Dagger2를 적용하도록 해보자.

[Dagger2 적용](../study_Android_Dagger2)
 

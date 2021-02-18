---
layout: post
author: study
title:  "Design Pattern - Observer Pattern"
description: "옵저버 패턴에 대해 알아보자"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---


### 개요

비동기(Async) 이벤트와 Observer 디자인 패턴이 핵심이라고 할수 있는 Reactive Programming는 최근 안드로이드 개발에서 선택이 아닌 필수가 되었다.

그렇기 때문에 Observer 디자인 패턴을 살펴봄으로서 더욱 다양한 안드로이드 동작에서 사용할수 있도록 해보자.

### Observer Pattern

잡지를 발행하는 잡지사와 그 잡지를 매달 구독하는 독자들을 생각해보자.
독자들은 잡지를 발행하는 잡지사와 `계약`을 맺고 매달 발행되는 잡지를 확인하고 읽는다. 이렇게 지속되다가 더 이상 잡지를 읽고 싶지 않을 경우 `계약 해지`를 하게 된다.

프로그램에서도 이러한 방식으로 처리를 하도록 만들수 있는데, 상태를 가지고 있는 주체 객체와 상태의 변경을 알아야 하는 관찰 객체(Observer Object)가 존재하며 이들의 관계는 1:1이 될수도, 1:N이 될수 있다. 이러한 형태가 옵저버 패턴이다.

이제 이러한 옵저버 패턴을 한번 만들어보자.


### 뉴스 구독(Observer 예시)

매일매일 새로운 뉴스를 제공하는 기계가 있고, 이 기계는 새로운 뉴스를 취합하여 정보를 가지고 있다. 그리고 이 기계에서 나온 뉴스를 구독하고 싶어하는 Observer 있다고 하자. 이들은 기계에서 새로운 뉴스를 생성할 때 마다 이 소식을 바로 받을 수 있다.

이를 프로그래밍 해보자면, 인터페이스 Publisher에서 Observer를 관리하는 Method를 가지고 있고 인터페이서 Observer는 정보를 업데이트 해주는 Update method를 가지게 된다.

이를 토대로 만든 클래스 다이어그램은 아래와 같다.

<div class="card h-100 my-u-padding"><div class="insertcover"><div class=""><img class="inserturl" src="{{ site.baseurl }}/assets/images/study/android/observerpattern.jpg" alt="by lazy"/></div></div></div>


간단히 살펴보자면,
Publisher를 구현한 NewsMachine 클래스는 정보를 제공하는 Publisher가 되며 Observer객체들을 가지고 있다. 그리고 Observer 인터페이스를 구현한 AnnualSubscriber(매년 정기구독자), EventSubscriber(이벤트 고객) 클래스는 NewsMachine이 새로운 뉴스를 notifyObserver()를 호출하면서 알려줄 대마다 Update가 호출된다.

이를 구현해보고 자세히 알아보자.


#### 1. Publisher Interface

```java
public interface Publisher {
    public void add(Observer observer);
    public void delete(Observer oberver);
    public void notifyObserver();
}
```

신문 발행 장치이다. 
이곳에는 읽고 싶어하는 사람과 계약을 맺고 끊을수 있는 add, delete가 존재하며,
그 계약자들에게 신문이 발행되었음을 알리는 notifyObserver가 존재한다.

#### 2. Observer Interface

``` java
public interface Observer {
    public void update(String title, String news);
}
```

이 인터페이스를 가지고 있어야 계약을 맺을수 있기 때문에 계약을 맺는 계약서이자 계약 시 신문 배달을 집 이나 직장에 해달라고 설정하는 장치라고 할수 있다.

#### 3. 신문 기계

```java
public class NewsMachine implements Publisher { 
    private ArrayList<Observer> observers; 
    private String title; 
    private String news; 
    
    public NewsMachine() { 
        observers = new ArrayList<>(); 
    } 
    
    @Override public void add(Observer observer) { 
        observers.add(observer); 
    }
    
    @Override public void delete(Observer observer) { 
        int index = observers.indexOf(observer); 
        observers.remove(index); 
    }
    
    @Override public void notifyObserver() { 
        for(Observer observer : observers) { 
            observer.update(title, news); 
        } 
    } 
    
    public void setNewsInfo(String title, String news) { 
        this.title = title; 
        this.news = news; 
        notifyObserver(); 
    } 
    
    public String getTitle() { 
        return title; 
    }
    
    public String getNews() { 
        return news; 
    } 
}
```

신문 기계에서 하는일은 다음과 같다. 

- 구독자를 관리하는 장부를 만든다.
- 신문 발행 장치에서 장비를 커스터마이징하여 장부에 입력하는 기능, 장부에 삭제하는 기능, 발행을 알려주는 기능을 작성한다.
- 이때, 발행을 알려주는 notifyObserver에서는 계약자들이 작성했던 발행 장소를 실행해준다.
- 뉴스를 발행하는 장치를 만든다. 이 장치가 실행되면 발행값을 저장하고, 발행을 알려주는 notifyObserver가 작동한다.



#### 4. 구독자 

2명의 구독자를 만들 예정이다.

구독자는 계약서를 가지고 있어야하며(Observer), 계약을 체결한다. 그리고 배달 장소를 입력해준다.

```java
public class AnnualSubscriber implements Observer { 
    private String newsString;
    private Publisher publisher;
    
    public AnnualSubscriber(Publisher publisher) { 
        this.publisher = publisher;
        publisher.add(this);
    } 
        
    @Override public void update(String title, String news) { 
        this.newsString = title + " \n -------- \n " + news; display();
    } 
        
    private void display() { 
        System.out.println("\n\n오늘의 뉴스\n============================\n\n" + newsString); 
    }

}

```

```java
public class EventSubscriber implements Observer { 
    private String newsString; 
    private Publisher publisher; 
    
    public EventSubscriber(Publisher publisher) {
        this.publisher = publisher; publisher.add(this); 
    } 
    
    @Override public void update(String title, String news) { 
        newsString = title + "\n------------------------------------\n" + news;
        display();
    }
    
    public void display() { 
        System.out.println("\n\n=== 이벤트 유저 ===");
        System.out.println("\n\n" + newsString);
    }
}
```

#### 5. 동작 화면

```java
public class MainClass { 
    public static void main(String[] args) {
        NewsMachine newsMachine = new NewsMachine();
        AnnualSubscriber as = new AnnualSubscriber(newsMachine); 
        EventSubscriber es = new EventSubscriber(newsMachine); 
        
        newsMachine.setNewsInfo("오늘 한파", "전국 영하 18도 입니다."); 
        newsMachine.setNewsInfo("벛꽃 축제합니다", "다같이 벚꽃보러~"); 
    } 
}
```

```
오늘의 뉴스
============================

오늘 한파
 -------- 
전국 영하 18도 입니다.


=== 이벤트 유저 ===


오늘 한파
------------------------------------
전국 영하 18도 입니다.


오늘의 뉴스
============================

벛꽃 축제합니다
 -------- 
다같이 벚꽃보러~


=== 이벤트 유저 ===


벛꽃 축제합니다
------------------------------------
다같이 벚꽃보러~
```


#### 끝


### 그렇다면 안드로이드에선 어떻게 동작하지?

앞서 공부한 예시를 안드로이드에서 동작하는 방식으로 바꿔보자.

안드로이드에선 값.observe(lifecycle, onChange) 형식으로 동작을 한다.



















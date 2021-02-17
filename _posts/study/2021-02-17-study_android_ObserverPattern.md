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

이를 토대로 클래스 다이어그램은 아래와 같다.

<div class="card h-100 my-u-padding"><div class="insertcover"><div class=""><img class="inserturl" src="{{ site.baseurl }}/assets/images/study/android/observerpattern.jpg" alt="by lazy"/></div></div></div>


자세히 살펴보자면,
Publisher를 구현한 NewsMachine 클래스는 정보를 제공하는 Publisher가 되며 Observer객체들을 가지고 있다. 그리고 Observer 인터페이스를 구현한 AnnualSubscriber(매년 정기구독자), EventSubscriber(이벤트 고객) 클래스는 NewsMachine이 새로운 뉴스를 notifyObserver()를 호출하면서 알려줄 대마다 Update가 호출된다.

이를 구현해 보자.

1. Observer Interface

``` java
public interface Observer {
    public void update(String title, String news);
}
```


2. Publisher Interface

```java
public interface Publisher {
    public void add(Observer observer);
    public void delete(Observer oberver);
    public void notifyObserver();
}
```









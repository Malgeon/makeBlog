---
layout: post
author: study
title:  "Kotlin - Lambda"
description: "그래서 람다를 뭐야, 어떻게 쓰지?"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---


### 개요

`let, with, apply, run`

코틀린을 접하면 공부해야 하는 확장함수이다. 
신입 개발자 면접에서도 각각의 함수를 구분 하는지 알아보는 질문으로 나올만큼 기본적이면서도 중요하다고 할수 있다.

공부하는 방법으로 `어떤 개념에 대해 달달 외우는 것은 도움이 되지 않는다.` 라고 생각하지만, 회사에 취직을 하고 싶으니 어쩌겠나 달달 외울 수밖에. 

막상 인터뷰 당시 떨려서 잘 대답하지도 못했지만(..) (그래서 달달 외우는건 싫다.)

<br><center>이제는 달달 외우는 것에서 벗어나보자.</center><br>

그렇지만 `let, with, apply, run`들에 대해 알고자 하는 포스팅은 아니며, 해당 확장함수를 구성하고 있는 `람다`에 대해 알아보고자 한다. 

그러면 `let, with, apply, run`를 이해하고 적용하는데 더욱 수월할 것이다.

### Preview

사실 람다식에 대해 궁금하게 된 계기는 옵저버 패턴을 다뤘던 이전 포스팅에서 liveData를 최대한 비슷하게 구현해보고자 했던 시도 중 발생한 이슈에서 출발한다. (확장 함수를 구성하고 있는 람다를 이해해서 확장함수를 이해하고 싶어서 출발한 것은 아니다!?)

해당 이슈는 `interface의 기능을 유지하면서 람다식을 적용하기 위한 방법`에 대한 것인데, 이전 포스팅에서는 적용하지 않고 익명 객체로 처리를 했지만 더욱 깔끔한 코딩을 위해 람다식을 적용하고자 한다면 아래와 같은 2가지 해결방안이 존재한다.

1. 해당 interface를 java로 만든다.

```java
public interface MyObserver {
    void update(String title, String news);
}
```

2. Kotlin의 버전이 1.4 이상인 버전에 한해서 interface 앞에 fun 키워드를 붙여준다.

```kotlin
fun interface MyObserver {
    fun update(title: String, news: String)
}
```

이 포스팅은 여기서 부터 출발 한다.

### Functional Interface - Lambda Expression














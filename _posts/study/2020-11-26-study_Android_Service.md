---
layout: post
author: study
title:  "Android Service"
description: "서비스"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/kotlin.png
---

** 201126 기준 공부 지식을 가지고 작성하는 노트
** 생존코딩 될 때까지 안드로이드 강의 참고


### 안드로이드에서 서비스가 필요한 경우

비동기 작업을 위해 스레드를 동작한 뒤 스레드 내의 동작하고 있는 값을 다루고자 하지만 해당 앱을 종료한 이후 재시작하게 될 경우에는 해당 액티비티에서 스레드 정보는 지워졌기 때문에 접근 할 수가 없다. 이때, 서비스는 접근이 가능하다.

위와 같은 목적을 가진 앱을 구현하기 위해선 서비스가 필요하다.
앱 설치 진행시작 후 해당 화면 나가더라도 앱 설치가 계속해서 진행하는 것이나, 음악 앱에서 앱을 나가더라도 음악이 여전히 재생되는 것을 예로 들수 있다.



### Service
동작을 하는 백그라운드 스레드를 구현 해야 하며, 스레드를 서비스 destory하게 될 때 null 값이 되므로 서비스를 통해 스레드 동작을 진행하게 되면 한번만 가능하게 된다. 또한 해당 서비스를 destroy하지 않으면 여전히 스레드는 남아있게 된다.


### IntentService -> JobIntentService
백그라운드 스레드가 필요없이 스레드와 동일한 동작을 할수 있으며, 해당 동작은 순차적으로 진행이 된다. 그리고 별다른 구문 없이 destory가 된다.

IntentService는 API 30 에서 deprecated 되었다. 대신 JobIntentService를 사용해야 한다.

비슷한 동작을 하나 제한시간이 존재하며(10분), 권한부여가 필수이다.


### ForegroundService
서비스를 동작을 시켜놓았지만, 다른 앱과의 메모리 충돌과 같은 여러 이슈로 안드로이드에서 해당 서비스를 Kill 할수 있다. 이때 ForGroundService를 사용하면 절대로 종료가 되지 않는다.

위와 같은 강력한 동작 때문에, 이 서비스를 사용하려면 제약이 존재한다.

1. ForegroundService가 동작하고 있음을 알려야 한다.(알림 아이디를 0이 아닌 값으로 설정)
2. StartForeground 메서드를 서비스 내부에서 별도로 실행한다. foreground로 승격을 시켜야 한다. 즉 Service에서 ForegroundSerivce를 Start를 해야 한다.


### BindService
다른 컴포넌트가 서비스 컴포넌트와 연결 고리를 만든다.

예를들어 음악앱과 같은 경우 서비스 단에서 음악을 실행하면 실행과 동시에 앱의 액티비티에서도 실행하고 있는 화면으로 변경되는 동작이 가능하도록 해준다.

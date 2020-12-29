---
layout: post
author: study
title:  "Android gradle - dependencies 관리"
description: "dependencies 관리를 더욱 용이하게 해보자."
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---


### Gradle

[Android Studio에서 제공하는 Gradle](https://developer.android.com/studio/build?hl=ko)

Gradle은 android studio에서 기본적으로 제공하는 빌드 배포 도구이다.
위의 링크를 통해 자세히 알수 있다.

여기서 하고 싶은 것은 프로젝트에 필요한 라이브러리를 모듈이 추가되더라도 한꺼번에 관리할 수 있고자 한다. 
이때, 기본값으로 설정된 build.gradle(project level)에서 관리하기 보다 추가로 project level의 versions.gradle을 만들어 관리하는 편이 용이하다.

<br><br>

### versions.gradle에 한번에 관리하기. 
<br>

우선 rootProject에서 versions.gradle 생성한다. 

file을 #.gradle로 만들면 자동으로 gradle build script가 된다.(이름이 versions가 아니어도 된다.)

이곳에 build.gradle(proejct level)의 repositories와 dependencies 그리고 build.gradle(module level)의 android version 및 dependencies를 관리하려 한다.


이를 위해선 알아야 할 groovy문법은 다음과 같다.
- 로컬 변수 : 'def 변수명' 으로 선언. 해당 변수는 로컬에서만 접근이 가능하다.
- ext 변수 : 프로젝트 전체와 서브 프로젝트에서도 접근이 가능하다.
- {} 사용 : 객체를 할당하고자 할 경우 사용한다.


proejct level에서 versions.gradle을 만들어 로컬 변수에서 각각의 라이브러리 version과 dependency를 관리하여 해당 로컬 변수를 ext에 할당하여 프로젝트 전체와 서브 프로젝트에서도 접근이 가능하도록 해준다.


방법은 아래와 같다.

- build.gradle(proejct level)의 buildscript에 versions.gradle을 사용할수 있도록 한다.

```java
// project level build.gradle
buildscript {
    apply from: 'versions.gradle'
    ...
}
```

- dependencies를 관리해주기 위해 empty map을 선언해준다.

```java
// project level versions.gradle
ext.deps = [:]
```

- version과 build_version을 empty map 로컬 변수로 선언을 하고 각각의 version을 할당해준 뒤, 다시 ext.version과 ext.build_version에 할당해 준다.
이때 version과 같이 많은 라이브러리를 다룰 경우 알파벳 순으로 정렬을 해두는 것이 좋다. (deps에서도 동일하다.)

```java
// project level versions.gradle
def versions = [:]
versions.kotlin = "1.3.72"
ext.versions = versions

def kotlin = [:]
kotlin.stdlib = "org.jetbrains.kotlin:kotlin-stdlib:$versions.kotlin"
kotlin.plugin = "org.jetbrains.kotlin:kotlin-gradle-plugin:$versions.kotlin"
deps.kotlin = kotlin
```

- 이제 versions.gradle에 있는 값을 각 dependencies에 적용을 해준다.

```java
// module level build.gradle
dependencies {
    implementation deps.kotlin.stdlib
}

// project level build.gradle
dependencies {
    classpath deps.kotlin.plugin
}
```


또한, 라이브러리를 가져오는 repositories를 역시 versions.gradle에 집어 넣을수 있다. 이를 이용하여 maven을 통해 추가로 필요한 라이브러리 또한 version.gradle에서 관리 할수 있다.

```java
// project level versions.gradle
def addRepos(RepositoryHandler handler) {
    handler.google()
    handler.jcenter()
}
ext.addRepos = this.&addRepos
```


```java
// project level build.gradle
buildscript {
    apply from: 'versions.gradle'
    addRepos(repositories)
    ...
}
allprojects {
    addRepos(repositories)
}
```


앞으로는 versions.gradle을 통하여 전체 모듈의 라이브러리를 관리하도록 하자.
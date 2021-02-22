---
layout: post
author: study
title:  "Design Pattern - Observer Pattern 2"
description: "옵저버 패턴을 깊게 다뤄보자.(feat. MediatorLiveData)"
categories: [ study ]
postImgOn: true
tags: [ android ]
image: assets/images/study/android/android.jpg
---


### 개요

이번엔 MediatorLiveData를 사용하여 Api response를 목적에 맞게 처리하는 모습을 설계해보자.

### 공부중 ...(!)

```kotlin
abstract class NetworkBoundResource<ResultType, RequestType>
@MainThread constructor(private val appExecutors: AppExecutors) {

    private val result = MediatorLiveData<Resource<ResultType>>()

    init {

        result.value = Resource.loading(null)
        @Suppress("LeakingThis")
        val dbSource = loadFromDb() // Room에서 data를 가지고 오는 함수(return값: LiveData)
        /*
        궁금한점 1. Room에서 Data를 LiveData로 가지고 올 시 Room 에서 쿼리를 해야 하는 동작을 하므로 
        loadFromDb()의 함수의 실행 뒤에 result.addSource(dbSource)를 하더라도 
        타이밍 상 dbSource는 liveData 자체의 초기값 -> 쿼리 data를 옵져빙이 가능하다는 것인가요?
        */
        result.addSource(dbSource) { data ->
            result.removeSource(dbSource)
            if (shouldFetch(data)) {
                fetchFromNetwork(dbSource)
            } else {
                result.addSource(dbSource) { newData ->
                    setValue(Resource.success(newData))
                }
                /*
                궁금한점 2. 여기서 init이 끝나게 되는데, 그렇다면 이 addSource 한번 dbSource를 통해 newData를 받고 나면 
                init이 끝나고 익명객체를 통해 동작되었기 때문에 해당 객체가 소멸됨과 동시에 result는 자동으로 해제가 되는 것인가요?
                */
            }
        }
    }
    /*
        궁금한점 . dbSource를 계속 가지고 와서 옵저빙 하는 이유가 가장 최근값을 빠르게 디스패치 하도록 함인데, 그래야 하는 이유를 모르겠습니다.
        가설
        dbSource인 liveData는 Dao의 쿼리 함수인데, 이는 Room의 Db를 옵저빙 하는 것이다. 따라서 앱의 다른 동작에서 db를 변경할 시 연동이 되어 값이 또 날라오는데
        가장 최근값을 디스패치한다는 이유는 그 이유이다.

        이게 사실이라면, Room의 함수를 liveData로 관리해 놓으면 Db자체가 바뀔때마다 해당 함수들이 연동되어 동작한다는 것을 염두하고 설계를 해야한다.
        물론 이 기능은 매우 좋은 기능이다. 라고 생각되는데 맞나요..?
    */

```
---
layout: post
author: study
title:  "Kotlin - Property"
description: "코틀린의 Property 접근 분석"
categories: [ study ]
postImgOn: true
tags: [ kotlin ]
image: assets/images/study/kotlin.png
---

### 개요

코틀린이 지원하는 Property 접근 방법을 알아보려고 한다.

[이 글은 이 포스트를 기반으로 작성되었다.](https://medium.com/til-kotlin-ko/kotlin-delegated-property-by-lazy%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80-74912d3e9c56)


### kotlin

코틀린은 null을 싫어한다. (개인적인 생각이다.)

```java
class MainActivity : AppCompatActivity() {
    private var helloMessage : String = "Hello"
}
```

그러나 아래와 같이 프로그램의 라이프사이클에서 초기화 이후 Property의 참조를 생성해야 하는 경우가 많다.

```java
public class MainActivity extends AppCompatActivity {
    private TextView mWelcomeTextView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mWelcomeTextView = (TextView) findViewById(R.id.msgView);
    }
}
```

물론 똑같이 만들수는 있다.

```kotlin
class MainActivity : AppCompatActivity() {
    private var mWelcomeTextView: TextView? = null
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        mWelcomeTextView = findViewById(R.id.msgView) as TextView
    }
}
```

그렇지만 앞서 말했듯 kotlin은 null을 싫어한다. 그래서 하나의 방법으로 lateinit을 지원한다.

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var mWelcomeTextView: TextView
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        mWelcomeTextView = findViewById(R.id.msgView) as TextView
    }
}
```

lateinit은 생성자 단계에서 값이 저장되지 않은 상태를 컴파일러가 인정하도록 한다.
따라서 컴파일 단계에서의 값과 처음 값에 접근할 때의 값이 다르므로 해당 프로퍼티는 var만이 사용할 수 있다.

그렇지만 위의 프로그램을 살펴보면 MainActivity의 라이프사이클동안 해당 참조는 동일하기 떄문에 read-only로 주어야 하지 않을까?


이를 위해 kotlin에서는 by lazy를 이용한 초기화 지연을 제공한다.

### by lazy

```kotlin
class MainActivity : AppCompatActivity() {
    private val messageView : TextView by lazy {
        // messageView의 첫 액세스에서 실행됩니다
        findViewById(R.id.message_view) as TextView
    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
    fun onSayHello() {
        messageView.text = "Hello"
    }
}
```

이제 by lazy의 동작을 살펴보고 이해해보자.


<div class="card h-100 my-u-padding"><div class="insertcover"><div class=""><img class="inserturl" src="{{ site.baseurl }}/assets/images/study/kotlin/kotlin_bylazy.png" alt="by lazy"/></div></div></div>


lazy를 통해 선언된 프로퍼티를 by가 위임하는 것을 알 수 있다.

그렇다면 lazy의 동작을 살펴보자. 

[우선 Kotlin 표준 라이브러리에선 이렇게 설명한다.](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/lazy.html)

1. lazy()는 람다를 전달받아 저장한 Lazy<T> 인스턴스를 반환한다.
2. 최초 getter 실행은 lazy()에 넘겨진 람다를 실행하고, 결과를 기록한다.
3. 이후 getter 실행은 기록된 값을 반환한다.

`즉, lazy는 프로퍼티의 값에 접근하는 최처 시점에 초기화를 수행하고 이 결과를 저장한 뒤 기록된 값을 재 반환하는 인스턴스를 생성하는 함수라는 것을 알 수 있다.`


설명을 이해하기 위해 lazy를 디컴파일 해보자.

```kotlin
class Demo {
    val myName : String by lazy { "John" }
}
```

```java
public final class Demo {
    @NotNull
    private final Lazy myName$delegate;
    
    // $FF: synthetic field
    static final KProperty[] $$delegatedProperties = ...
    @NotNull
    public final String getMyName() {
        Lazy var1 = this.myName$delegate;
        KProperty var3 = $$delegatedProperties[0];
        return (String)var1.getValue();
    }
    public Demo() {
        this.myName$delegate =
            LazyKt.lazy((Function0)null.INSTANCE);
    }
}
```

코드를 살펴보자

- myName에 &#36;delegate를 붙인 필드(myName&#36;delegate)를 생성한다. 
- myName&#36;delegate 타입은 String이 아닌 Lazy이다.
- 생성자에서 myName&#36;delegate에 대하여 LazyKt.lazy()를 할당.
- LazyKt.lazy()는 주어진 초기화 블록을 실행하는 역할을 한다.

실제 동작은 getMyName()의 호출 시 Lazy로 선언된 myName&#36;delegate를 가져와 getValue()를 이용하여 초기화 된 값을 가져온다.

`$$delegatedProperties는 위임된 프로퍼티들의 정보(owner class, 필드명, accessor, modifier, getter의 named signature 등)를 가지고 있는 KProperty[] 타입의 필드이다.`


[더 많이 알아보고 싶다면 이글을 참고하자.](https://medium.com/til-kotlin-ko/kotlin-delegated-property-by-lazy%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80-74912d3e9c56)





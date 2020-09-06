---
layout: post
author: study
title:  "우선순위 큐"
description: "우선순위 큐 다루기"
categories: [ study ]
tags: [programming, kotlin]
postImgOn: true
image: assets/images/study/kotlin.png
---

# 우선순위 큐
max heap 또는 min heap 이라고도 할 수 있는 우선순위 큐를 코틀린에서는 지원을 해준다.

이런 우선순위 큐를 알아보자.

```
import java.util.PriorityQueue
```
```java
fun main() {
  val queue1 = PriorityQueue<Int>() 
  val queue2 = PriorityQueue(Comparator.reverseOrder<Int>())
}
```

PriorityQueue를 사용하기 위해서는 java.util.PriorityQueue(java.util.*)가 필요하다. <br>
기본값으로는 값이 낮을 수록 우선순위가 높게 설정되어 있다. 이를 값이 높을 수록 우선순위가 높도록 바꾸기 위해선 queue2처럼 선언하면 된다.


사용가능한 함수로는 아래와 같다.

| 함수 | 기능 |
| -- | -- |
| add | 추가 |
| offer | 추가 |
| size | 크기 |
| clear | 모두 지우기 |
| peek | 가장 앞부분 가져오기(해당 값 복사) |
| poll | 가장 앞부분 빼오기(해당 값 잘라내기) |
| isEmpty | 비어있는지 확인 |


### add 와 offer의 차이

AbstarctQueue일때 

```java
public boolean add(E e) {
    if (offer(e))
        return true;
    else
        throw new IllegalStateException("Queue full");
}
```


### 사용 예

```java
fun main(args: Array<String>) {
  val nums: PriorityQueue<Int> = PriorityQueue<Int>()

  // Add items (enqueue)
  nums.add(800)
  nums.add(50)
  nums.add(200)
  nums.add(550)

  println("peek: " + nums.peek())
  
  // Remove items (dequeue)
  while (!nums.isEmpty()) {
    println(nums.remove())
  }
}
```

Output: 

```
peek: 50
50
200
550
800
```


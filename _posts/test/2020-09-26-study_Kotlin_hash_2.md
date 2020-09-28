---
layout: post
author: test
title:  "해시 : 호텔 방 배정"
description: "2019 카카오 개발자 겨울 인턴십"
categories: [ test ]
tags: [ hash ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/64063"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">해시 : 호텔 방 배정</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


해시 충돌 문제 처리를 얼마나 효율적으로 할 수 있도록 만드는 문제이다.
문제가 BFS나 이분탐색과 같은 전형적인 문제가 아니었기 때문에 생각보다 많이 어려웠다.
그래서 우선적으로 완전 탐색으로 문제를 풀고, 해당 풀이를 뼈대로 더 효율적으로 할 수 있는 방법을 찾아보았다.

- 완전 탐색 방법 
    당연히 정확성은 100%, 효율성은 14%(7개중 1개 통과)

```java
class Solution {
    fun solution(k: Long, room_number: LongArray): LongArray {
        var answer = arrayListOf<Long>()
        val used = Array(k.toInt()+1) { false }
        fun findRoom(idx: Long) {
            var intIdx = idx.toInt()
            if(intIdx === used.size) return
            if(!used[intIdx]) {
                used[intIdx] = true
                answer.add(idx)
            }
            else findRoom(idx+1)
        }
        room_number.forEach {
            findRoom(it)
        }
        return answer.toLongArray()
    }
}
```

효율적으로 하기 위한 방법은 
1. 원하는 방이 이미 차있다면 다음 비어있는 번호에 대한 정보를 가지고 있어야한다.
2. 해당 방에 대한 정보를 가지는 방법은 해시를 이용항 방법과 재귀를 이용한 방법이 있다.


- 재귀를 이용한 방법

```java
class Solution {
    fun solution(k: Long, room_number: LongArray): LongArray {
        val used = mutableMapOf<Long, Long>()
        return room_number.map { findRoom(it, used) }.toLongArray()
    }

    fun findRoom(room: Long, used: MutableMap<Long, Long>): Long {
        used[room]?.let {
            var next = findRoom(it, used)
            used[room] = next + 1
            return next
        } ?: run {
            used[room] = room + 1
            return room
        }
    }
}
```

- 해시를 이용한 방법

null처리를 명확히 해줘야 해서, 조금 복잡하게 코딩처리가 되었다.

```java
class Solution {
    fun solution(k: Long, room_number: LongArray): LongArray {
        var answer = arrayListOf<Long>()
        val used = mutableMapOf<Long, Long>()

        fun findRoom(idx: Long) {
            used[idx]?.let {
                var item = it
                while(item <= k) {
                    used[item]?.let { target ->
                        used[target]?.let { nextTarget ->
                            used[target] = nextTarget+1
                            item = nextTarget
                        } ?: run {
                            used[target] = target+1
                            used[idx] = target+1
                            answer.add(target)
                            return
                        }
                    } ?: run {
                        used[item] = item+1
                        used[idx] = item+1
                        answer.add(item)
                        return
                    }
                }
            } ?: run {
                used[idx] = idx+1
                answer.add(idx)
            }
        }
        room_number.forEach {
            findRoom(it)
        }
        return answer.toLongArray()
    }
}
```
---
layout: post
author: test
title:  "설계 : 키패드 누르기"
description: "2020 카카오 인턴십"
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/67256"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 키패드 누르기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

왼쪽, 오른쪽의 Starting Point이 존재하고 조건에 따라 해당 Point는 바뀌는데, 조건은 바꾸려는 Point와 양쪽 의 거리 차이이다.
따라서 배열을 구성해서 해당 Point간 거리를 구할수 있어야 하며, 움직일때마다 Point를 저장하고 있어야 한다.

아래는 다음과 같은 설계를 통해 구성하였다.
0. 각 숫자에 맞는 배열을 구성하여 해당 위치를 배열을 기준을 찾도록 한다.
1. 오른손과 왼손의 현재 위치를 Location이란 Data class에 담는다.
2. Data class는 comparable을 상속하여 위치간 거리를 계산할수 있도록 한다.
3. 매개값으로 들어오는 숫자에 해당하는 배열의 위치를 구하도록 함수를 구성한다.
4. 조건에 맞으면 현재 위치를 다음 위치로 변경한 후 record에 움직인 값을 기록한다.

```java
class Solution {
  data class Location(val x: Int, val y: Int) : Comparable<Location> {
    override fun compareTo(other: Location): Int {
      return abs(this.x - other.x) + abs(this.y - other.y)
    }
  }

  val keypad = arrayOf(
    intArrayOf(1,2,3),
    intArrayOf(4,5,6),
    intArrayOf(7,8,9),
    intArrayOf(-1,0,-1)
  )

  var left = Location(0, 3)
  var right = Location(2, 3)

  fun solution(numbers: IntArray, hand: String): String {
    var record = ""
    numbers.forEach {
      when(it){
        1, 4, 7 -> record += move("left", it)
        3, 6, 9 -> record += move("right", it)
        else -> {
          val leftOrRight = distance(findLocation(it), left, right)
          when {
            leftOrRight == 0 -> {
              if (hand == "right") {
                record += move("right", it)
              } else {
                record += move("left", it)
              }
            }
            leftOrRight < 0 -> record += move("left", it)
            else -> record += move("right", it)
          }
        }
      }
    }
    return record
  }

  fun move(hand: String, target: Int): String {
    if (hand == "left") {
      left = findLocation(target)
      return "L"
    } else {
      right = findLocation(target)
      return "R"
    }
  }

  fun findLocation(target: Int): Location {
    for (i in 0..3) {
      if (keypad[i].contains(target)) {
        return Location(keypad[i].indexOf(target), i)
      }
    }
    return Location(0, 0)
  }

  fun distance(target: Location, left: Location, right: Location): Int 
    = left.compareTo(target) - right.compareTo(target)
}
```

또 다른 풀이로
아래는 다음과 같은 설계를 통해 구성하였다.
0. 각 숫자에 맞는 위치를 Collection Map에 Pair로 지정하여 구성한다.   
1. 오른손과 왼손의 현재 위치를 Pair로 지정한다.
2. abs를 통해 거리를 구한다.
3. map의 결과를 joinToString을 이용해 결과값으로 만든다.



```java
class Solution {
    val info = mapOf ( 0 to Pair(1,3),
            1 to Pair(0,0),
            2 to Pair(1,0),
            3 to Pair(2,0),
            4 to Pair(0,1),
            5 to Pair(1,1),
            6 to Pair(2,1),
            7 to Pair(0,2),
            8 to Pair(1,2),
            9 to Pair(2,2)
    )
    
    fun solution(numbers: IntArray, hand: String): String {
        val lefthanded = hand == "left" // boolean 만들기!
        var nowLeft = Pair(0,3)
        var nowRight = Pair(2,3)
        
        return numbers.map { value ->
            info[value]?.let {
                if(it.first == 0) {
                    nowLeft = it
                    'L'
                } else if(it.first == 2) {
                    nowRight = it
                    'R'
                } else {
                    val distLeft = abs(it.first-nowLeft.first) 
                                    + abs(it.second-nowLeft.second)
                    val distRight = abs(it.first-nowRight.first) 
                                    + abs(it.second-nowRight.second)
                    if(distLeft > distRight) {
                        nowRight = it
                        'R'
                    } else if(distLeft < distRight) {
                        nowLeft = it
                        'L'
                    }else {
                        if(lefthanded) {
                            nowLeft = it
                            'L'
                        }else {
                            nowRight = it
                            'R'
                        }
                    }
                }
            }
        }.joinToString("")
    }
}
```
---
layout: post
author: test
title:  "Math 활용 예제"
description: ""
categories: [ test ]
tags: [ standard ]
postImgOn: false
image: assets/images/test/programmers.png
insertUrlImg: assets/images/study/programmers.png
---

### Math

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12934"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 정수 제곱근 판별</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


kotlin.Math를 사용하는 방법과 정수인지 판단하는 방법을 잘 숙지해 두자.
```
double % 1.0(double!) == 0.0(double!)
Math.pow(sqrt + 1, 2.0(double!))
```
```java
class Solution {
    fun solution(n: Long): Long {
        val sqrt = Math.sqrt(n.toDouble())
        return if(sqrt % 1.0 == 0.0) {
            Math.pow(sqrt + 1, 2.0).toLong()
        } else {
            -1L
        }
    }
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12940"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 최대 공약수와 최소 공배수</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>


최대 공약수의 2부터 두 숫자 중 작은 숫자까지 둘다 나누어 떨어지는 값이 있다면 그중 가장 큰 값에 해당하며, 없다면 1이게 된다.
최대 공배수는 두 숫자의 곱에 최대 공약수로 나눈 값에 해당한다.

```java
class Solution {
    fun solution(n: Int, m: Int): IntArray {
        var gcd: Int = 1
        for(i in Math.min(n, m) downTo 2) {
            if(n % i == 0 && m % i == 0) {
                gcd = i
                break
            }
        }
        return intArrayOf(gcd, m*n/gcd)
    }
}
```

재귀

```java
fun gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
```

a와 b가 있을때, a와 b의 최대공약수를 구하면 a를 b로 나눈 나머지를 구하고,
해당 나머지를 다시 b로 나눈 나머지를 구하며 나누어 떨어질때까지 진행한다.
a와 b의 크기에 따른 순서는 상관이 없다. 유클리드 호제법을 통한 최대공약수를 구한는 방법이다.

이를 [꼬리 재귀 함수](../study_kotlin_5)를 사용해 해결하였다.

```java
class Solution {
    fun solution(n: Int, m: Int): IntArray {
        val gcd = gcd(n, m)

        return intArrayOf(gcd, n * m / gcd)
    }

    tailrec fun gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
}
```

<div class="card h-100 my-u-padding"><div class="insertcover"><a target="_blank" class="text-dark" href="https://programmers.co.kr/learn/courses/30/lessons/12944"><div class=""><img class="inserturl" src="{{site.baseurl}}/{{ page.insertUrlImg}}" alt="programmers.co.kr"/></div><div class="insert-img-body"><h4 class="insert-img-title">연습문제 : 평균 구하기</h4><p class="insert-img-description">programmers.co.kr</p></div></a></div></div>

배열의 평균을 구해준다. 

```java
class Solution {
    fun solution(arr: IntArray) = arr.sum().toDouble()/arr.size
}
```

average() 함수가 지원이 된다. (Double return)

```java
class Solution {
    fun solution(arr: IntArray) = arr.average()
}
```


### 소수 찾기

```java
fun isPrimeNumber(num: Int): Boolean {
    return (2..num / 2).filter { num % it == 0 }.count() == 0
  }
```
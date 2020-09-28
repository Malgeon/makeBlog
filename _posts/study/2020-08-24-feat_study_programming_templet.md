---
layout: post
author: study
title:  "Kotlin 코딩테스트 템플릿"
description: "문제를 풀때 참고할만한 정보"
categories: [ study ]
postImgOn: false
tags: [programming, kotlin]
image: assets/images/study/kotlin.png
featured: true
hidden: true
study: true
---

### 코틀린 기본
코틀린에서 기본적인 동작을 예제에서 확인해보자.

- [배열 예제](../study_Kotlin_array_1)
- [문자열 예제](../study_Kotlin_string_1)
- [반복문 예제](../study_Kotlin_loop_1)
- [수식 예제](../study_Kotlin_math_1)
- [재귀 예제](../study_Kotlin_recursive_1)
- [논리 연산자 예제](../study_Kotlin_logicaloperator_1)

- [find와 first 예제](../study_Kotlin_bf_1)

### 배열
배열 선언과 출력하는 부분을 잘 숙지해 두자
- [배열](../study_Kotlin_25)
- [배열의 선언](../study_Kotlin_26)

[배열 예제](../study_Kotlin_stack&queue_2)


### 배열 정렬
기본적인 함수라고 할수 있지만, 자주 쓰이므로 따로 구분해 놓는다.
- [배열의 정렬](../study_Kotlin_27)

[배열의 정렬 예제](../study_Kotlin_design_3)
[배열의 정렬 예제](../study_Kotlin_sort_1)


### 표준 함수
함수형 프로그래밍을 하도록 코틀린에서 지원하는 여러 함수 들을 잘 숙지하고 이용하자.

- [closure, let, also, apply](../study_Kotlin_9)
    - [let을 이용한 Null 검사](../study_Kotlin_hash_2)
- [run, with, use, takeIf, takeUnless](../study_Kotlin_10)

[표준 함수의 이용 예제](../study_Kotlin_standardfunction_1)

### 확장 함수
함수형 프로그래밍을 하도록 코틀린에서 지원하는 여러 함수 들을 잘 숙지하고 이용하자.

- [forEach, reduce, fold](../study_Kotlin_33)
    - [reduce, fold 응용](../study_Kotlin_dfs_1)
- [map, groupBy, sequence](../study_Kotlin_34)


### Collection & 해시
Collection은 Data의 입력, 출력에서 강력한 기능을 가지고 있기 때문에 자주 쓰인다.
해당 기능 수행 후 toIntArray() 와 같이 원하는 자료형 형태로 return이 가능하게 해준다. 

- List : <br>
    - [Collection List-1](../study_Kotlin_29)
    - [Collection List-2](../study_Kotlin_30)
- Set : <br>
    - [Collection Set](../study_Kotlin_31)
- Map : <br>
    - [Collection Map](../study_Kotlin_32)

[Collection 관련 예제](../study_Kotlin_standardfunction_1)

해당 Collection - Set, Map를 이용해 해시문제를 풀수 있다.
[해시 관련 예제](../study_Kotlin_hash_1)
[해시 관련 예제](../study_Kotlin_hash_2)

### 조합, 순열. 멱집합
- [조합](../study_Programming_combination)
- [순열](../study_Programming_permutation)
- [멱집합](../study_Programming_powerset)

### 그래프
DFS, BFS에서 주로 사용된다. 
방향, 가중치를 이용한 문제도 출시 된적이 있으니 잘 숙지해두자.

- [그래프](../study_Programming_Graph)

### BFS/DFS
어려운 문제로 자주 사용되는 알고리즘.

- [BFS/DFS](../study_Programming_BFS&DFS)

### 스택/큐
- [큐 예제](../study_Kotlin_stack&queue_3)

### 우선순위 큐
코틀린은 자바와 같이 우선순위 큐를 지원해 준다.

[우선 순위 큐 기본](../study_Programming_priorityQueue)
- [우선 순위 큐 예제](../study_Kotlin_stack&queue_1)


### 이분탐색
어려운 문제로 등장하기 쉬운 문제 중 하나 이다.

기본 컨셉은 매우 간단해서 단순히 이분 탐색을 이용해 조건에 맞는 값을 찾는 것을 구하는 것보다 upper/lower bound를 이용하여 조건에 맞는 가장 작은 값을 찾는 문제로 변별력을 갖추려는 것 같다.

- [이분 탐색 예제 - upper/lower bound easy](../study_Kotlin_binarySearch_1) 
- [이분 탐색 예제 - upper/lower bound hard](../study_Kotlin_binarySearch_2)


### Greedy
사고력과 순발력을 요구를 하기 때문에 여러워지기 쉬운것 같다. 
그런데 해당 알고리즘의 한계점인 100% 최적해를 보장하지 않으므로, 문제를 낼 때 어느정도의 최적해를 정답으로 구분할지 판단하기 어려워진다.
간단하게 100% 최적해가 만들어 지지 않는 이상 굳이 문제로 내지 않을 것으로 생각이 된다.

- [Greedy 예제1 : 간단하게 최적해 보장](../study_Kotlin_greedy_1)
- [Greedy 예제2 : 최적해를 보장하기 어려움](../study_Kotlin_greedy_2)

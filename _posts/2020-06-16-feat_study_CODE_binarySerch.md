---
layout: post
author: study
title:  "이진탐색 : Upper Bound, Lower Bound"
description: "연습 문제"
categories: [ study ]
tags: [programming, javascript]
image: assets/images/study/algorithm.jpg
featured: true
hidden: true
study: true
---

# 이진탐색
  이진 탐색은 기본적으로 매우 중요한 개념이다. 
  특히 수학적인 부분에서 미적분과 같은 계산하기 어려운 값을 근사치로 나타내고자 할때, 주로 쓰이는 방법 중 하나라는 점에서 말이다. (수치해석 과목에서 잘 찾아볼수 있다.)

  수학적인 부분에서 잘 쓰인다면 역시 프로그래밍에서도 잘 쓰일 수 있는데, 이유는 방대한 양의 데이터를 찾고, 저장하고, 이용하고자 할때 소요되는 시간 (Big-O)에서 큰 활약을 할수 있기 때문이다.

  그렇기 때문에 숙지하고 문제해결에 있어서 잘 사용하도록 하자.


# 기본 개념
 
  정렬된 배열에서 처음값과 끝값을 잡은 다음 (보통 처음 : 0, 끝 : 배열의 끝)
  중간값을 (처음값 + 끝값)/2 로 잡는다.

  중간값에 해당하는 값을 찾은 뒤 해당값이 찾고자 하는 값과 비교하여 크거나 작을때 처음값 또는 끝값을 중간값으로 조정하면서 다시 중간값을 잡는다.

  값을 찾거나 처음값이 끝값보다 커질때까지 반복

```javascript
function binarySeach(list, key) {
  let low = 0;
  let high = list.length-1;
  let mid;

  while(low <= high) {
    mid = parseInt((low + high)/2);

    if(key>list[mid]) {
      low = mid+1;
    } else if (key<list[mid]) {
      high = mid-1;
    } else {
      return mid;
    }
  } 
  return -(low+1);
}
```

 # 찾고자 하는 값이 여러개일 경우

  이 문제를 해결하기위해 Upper_Bound와 Lower_Bound가 필요하다. 물론 기본 개념에서 약간 조정한 정도이나, 어쨌든 다르다.
  C++같은 경우 stl에서 내장된 메소드로 지원을 해주지만 자바에서는 제공을 해주지 않기 때문에 여기서 구현해 놓고자 한다.

  Upper_Bound와 Lower_Bound를 구분짓는 장치는 key값과 찾고자 하는 값이 같을 경우 처리방법이다.
  Upper 같은 경우 해당 값이 나오면 mid+1값으로 low값을 정하며
  Lower 같은 경우 mid 값을 high값으로 정한다.

  잘 생각해 보면, 이 차이로 Upper는 key 값보다 큰 직후값을 Lower는 key값이 시작되는 값을 갖게된다.
    (찾고하 하는 값의 index : Lower <= index < Upper)
    
 - Lower_Bound

```javascript
function lowerBound(list, key) {
  let low = 0;
  let high = list.length-1;
  let mid;

  while(low < high) {
    mid = parseInt((low + high)/2);

    if(key <= list[mid]) {
      high = mid;
    } else {
      low = mid+1;
    }
  } 
  return low;
}
```

 - Upper_Bound

```javascript
function upperBound(list, key) {
  let low = 0;
  let high = list.length-1;
  let mid;

  while(low < high) {
    mid = parseInt((low + high)/2);

    if(key >= list[mid]) {
      low = mid+1;
    } else {
      high = mid;
    }
  } 
  return low;
}
```


 # 샘플
```javascript
 /*
 * Javascript version of C++ equal_range via lower_bound and upper_bound
 *
 * Input: The array A in ascending order and a target T
 * Output: The tightly bound indices [i, j] where A[i] <= T < A[j] if T exists in A
 * otherwise [-1, -1] is returned if T does not exist in A
 */

let lowerBound = (A, T) => {
    let N = A.length,
        i = 0,
        j = N - 1;
    while (i < j) {
        let k = Math.floor((i + j) / 2);
        if (A[k] < T)
            i = k + 1;
        else
            j = k;
    }
    return A[i] == T ? i : -1;
};

let upperBound = (A, T) => {
    let N = A.length,
        i = 0,
        j = N - 1;
    while (i < j) {
        let k = Math.floor((i + j + 1) / 2);
        if (A[k] <= T)
            i = k;
        else
            j = k - 1;
    }
    return A[j] == T ? j + 1 : -1;
};

let equalRange = (A, T) => {
    return [lowerBound(A, T), upperBound(A, T)];
};
```


 # C++ Std: Upper_Bound 와 Lower_Bound를 구현해 보자

 숫자로만 된 배열 같은 경우 그냥 위에 있는것을 써도 되는데,
 예를 들어 length와 value를 함께 비교해야 하는 경우 위의 배열은 통하지가 않는다. (이진탐색문제 3 참조)
 그런데 C++ Std에서 제공하는 Upper_Bound와 Lower_Bound는 다중 제한 조건의 탐색이 가능한데, 
 자바에서는 제공하지 않는다. 따라서 구현해서 사용하자.


```javascript
var test = [10, 10, 10, 20, 20, 20, 30, 30, 40, 40, 40];
var lo = lower1Bound(test,0,10,40);
var up = upper1Bound(test,0,10,40);

console.log("lower: " + lo + ",  upper: " + up);


function lowerBound(list, start, end, key) {
  let it;
  let step;
  let advance;
  let first = start;
  let count = end - start + 1;
  //debugger;

  while (count > 0){
    step = Math.floor(count/2);
    advance = first+step;
    it = list[advance];
    if(it < key && it !== undefined) { //advance의 값이 0보다 작게 되면 false를 가리켜야 한다. 
      first = ++advance;
      count -= step+1;
    } else {
      count = step;
    }
  }
  return first;
}

function upperBound(list, start, end, key) {
  let it;
  let step;
  let advance;
  let first = start;
  let count = end - start + 1;
  //debugger;
  while (count > 0){
    step = Math.floor(count/2);

    advance = first+step;
    it = list[advance];
    if(!(key < it) && it !== undefined) {
      first = ++advance;
      count -= step+1;
    } else {
      count = step;
    }
  }
  return first;
}

```
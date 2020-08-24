---
layout: post
author: study
title:  "자바스크립트"
description: "자바 스크립트 공부 기록"
categories: [ study ]
tags: [programming, javascript]
---


# 숫자 -> 문자
 1. 값을 String으로 변환.
  
  ```javascript
  var numValue = 1;
  String(numValue);
  ```

 2. 값에 ''를 추가
 ```javascript
  var numValue = 1;
  numValue = numValue + '';
 ```

# 배열에 객체 넣기
 ```javascript
  var data = [];
  var key = "key" //이때 key값의 숫자로 구성되어 있을경우 data는 숫자로 판단한다.
  data[key] = anyValue;
 ```

# prototype.sort();
 매개변수가 없으면 문자열로 정렬.
 ```javascript
  prototype.sort( (a, b) => a-b );
 ```

 객체를 이용할 경우
 ```javascript
  var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic', value: 13 },
  { name: 'Zeros', value: 37 }
 ];

// value 기준으로 정렬
items.sort(function (a, b) {
  if (a.value > b.value) {
    return 1;
  }
  if (a.value < b.value) {
    return -1;
  }
  // a must be equal to b
  return 0;
});
 ```
   sort의 return 값이 1이면 바꿈, -1이면 바꾸지 않는다.
  
 # sort()
  sort를 그냥 두면 문자열 정렬이다 (10 < 2)
   따라서 숫자 정렬시 무조건 sort((a,b) => a-b); 꼭 쓰자

   
 # forEach, reduce, filter, map
   forEach => return x
   reduce => return o (한줄시 생략)
   map => return o (한줄시 생략)

   filter => return true 이면 가지고 옴.
 
 
  
  # ... 문법
   배열 할당
 ```javascript
[a1, a2, ...rest_a] = [1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log(a1); // 1
console.log(a2); // 2
console.log(rest_a); // [3, 4, 5, 6, 7, 8, 9]


var { a1, a2, ...rest_a } = { a1 : 10, a2 : 20, a3 : 30, a4 : 40 };
console.log(a1); // 10
console.log(a2); // 20
console.log(rest_a); // { a3: 30, a4: 40 }
 ```

# Number()
   문자열을 숫자로 -> 바꿀수 없을경우 NaN

# Set()
```javascript
  const foo = new Set();
  foo.add({}); // 빈 객체 추가
  foo.add({}); // 또 다른 빈 객체 추가
  console.log(foo.size); // 2
  console.log(foo) // Set { {}, {} }
  foo.add(10); // 숫자 10 추가
  foo.add('10'); // 문자열 "10" 추가
  foo.add(10); // 이미 10이 있기때문에 무시됩니다
  console.log(foo.size); // 4
  console.log(set); // Set { {}, {}, 10, '10' }
```
  prototype.has(값) — set에 값 존재 확인. true 또는 false를 반환.
  prototype.delete(값) — set에서 값을 제거.
  prototype.clear() — set에서 모든 값을 제거.

```javascript
  const foo = new Set([ '아이폰XR', '갤노트9' ]);
  const fooInArray = [ ...foo ];
  console.log(fooInArray) // [ '아이폰XR', '갤노트9' ];
```


# Array.splice();
 배열의 기존 요소를 삭제 또는 교체
 
 ```javascript
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb'); //1번 index에 삭제 없이 값 추가
console.log(months); 
// expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May'); // 4번 index에 1개 삭제 후 값 추가
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "May"]
```

# or 연산자

  값이 들어오지 않을 경우 초기값을 설정하고자 할때 다음과 같이 해보자.

```javascript
const initValue = (value) => theTitle || "initial Vlaue"; 
```

# 문자열 합치기

  단순히 더해주면 합쳐진다.

```javascript
"1" + "2" // "12"
```

# 배열 합치기

```javascript
const alpha = ['a', 'b', 'c'];

alpha.concat(1, [2, 3]);
// 결과: ['a', 'b', 'c', 1, 2, 3]
```

# 중복 값 제거하기
 1. Set() 이용
```javascript
prototype = new Set();
prototype.add( key );
[...prototype.keys()];
```
 2. 객체 이용
```javascript
prototype[ key ] = true;
Object.keys( prototype );
```




 # JSON.parse(text[, reviver])

  배열 및 객체문법에 속한 Text문을 배열 및 객체로 변환해준다.
```javascript
const json1 = '{"result":true, "count":42}';
const json2 = "[[1,2,3],[2,1],[1,2,4,3],[2]]";
const obj1 = JSON.parse(json1);
const obj2 = JSON.parse(json2); 
```

 # String.prototype.replace()
   String 안에 있는 값을 찾아 원하는 값으로 바꿔준다.
   문자열 리턴!

    g : 발생할 모든 pattern에 대한 전역 검색
    i : 대/소문자 구분 안함

```javascript
const p = 'The quick brown fox jumps over the lazy dog. If the dog reacted, was it really lazy?';

const regex = /dog/gi;

console.log(p.replace(regex, 'ferret'));
// expected output: "The quick brown fox jumps over the lazy ferret. If the ferret reacted, was it really lazy?"

console.log(p.replace('dog', 'monkey'));
// expected output: "The quick brown fox jumps over the lazy monkey. If the dog reacted, was it really lazy?"

```

 # 배열 n개로 나누기

 ```javascript
Array.prototype.division = function (n) {
  var arr = this;
  var len = arr.length;
  var cnt = Math.floor(len / n);
  var tmp = [];

  for (var i = 0; i <= cnt; i++) {
    tmp.push(arr.splice(0, n));
  }

  return tmp;
}
```
```javascript
const division = (arr, n) => {
  var len = arr.length;
  var cnt = Math.floor(len / n);
  var tmp = [];

  for (var i = 0; i <= cnt; i++) {
    tmp.push(arr.splice(0, n));
  }
  return tmp;
}
```

# matrix row -> column 전환
 - 한줄 전환
```javascript
var row = [0, 0, 0, 4, 3];
var result = [];
result = row.map( (_, i) => [...(result[i] || []), row[i]]);
/*
row -> column  [0, 
                0, 
                0, 
                4, 
                3]
*/ 

```

 - 전체 시계반대방향 전환
```javascript
const transpose = (matrix) =>
  matrix.reduce(
  (result, row) => row.map((_, i) => [...(result[i] || []), row[i]]),
  []
);
```

 - 전체 시계방향 전환
```javascript
function rotation(key) {
    let result = [];
    debugger;
    for(let i=key.length-1; i>=0; i--) {
        result = key[i].map( (_, $) => [...(result[$] || []), key[i][$]]);
    }
    return result; 
}
```


# 디폴트 매개변수
 디폴트 매개변수를 설정함으로써 함수 밖에서 일부로 설정을 안해줘도 되게 한다.
```javascript
function multiply(a, b = 1) {
  return a*b;
}

multiply(5); // 5
multiply(5, 2); // 10
```

 # nXn matrix 생성
```javascript
 new Array(n).fill(0).map( () => Array(n).fill(0));
 ```

# 문자열 빼기
 str은 변경되지 않는다.
```javascript
 var str = "abcd";
 var removeFirst = str.substring(1);
```
 
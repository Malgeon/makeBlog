---
layout: post
author: study
title:  "순열과 조합"
description: "순열과 조합에 대해서"
categories: [ Study ]
tags: [programming, javascript]
---
# 순열과 조합
 순열과 조합은 여러 군데에서 쓰인다. 
  쓰이는 방식도 다양한데, 
   - 각각의 배열의 값으로써
   - 조합된 숫자로써

    하지만 설계하는게 쉬운일이 아니기 때문에, 배열로 사용할수 있는 그리고 조합된 숫자를 사용하도록 String으로 사용하는 각각의 함수를 만들어 놓고자 한다.


# nPr, nCr 값
  
  수학이다.
  nPr => n개의 경우를 r번 사용할경우.
  nCr => nPr에서 중복 제외

```javascript
function combination(n, r) {
  let top = 1;
  let bottom = 1;

  for(let i=0; i<r; i++) {
    top *= n-i;
    bottom *= r-i;
  }

  return top/bottom;
}

function permutation(n, r) {
  let value = 1;

  for(let i=0; i<r; i++) {
    value *= n-i;
  }

  return value;
}
```


# 배열의 원소로 만들수 있는 가능한 모든 조합

  자주 나오는 문제이다. 기능을 추가해서 해결해야할 문제들이니 잘 알아두자. 
```javascript
//list 값은 문자열!
/**
* input은 배열, output은 배열의 순열조합으로 이루어진 String
*/
var myList = [1, 2, 3, 4];
var test = findAllPermutation(myList);
function findAllPermutation(list, prevValue) {
  const frontPaddingValue = prevValue || '';
  return list.reduce((listNumbers, value, index) => {
    listNumbers.push(frontPaddingValue+value);

    const nextNumberList = [...list];
    nextNumberList.splice(index, 1);

    const result = findAllPermutation( 
    nextNumberList, frontPaddingValue + value
    );
    listNumbers.push(...result);

    return listNumbers;
  }, []);
}
```
 잘 생각해 보자. 배열 내 각각의 요소를 저장한 그 전값과 합쳐주면 된다. 단 각각의 요소를 연산할 때 동일한 조건을 가지도록 해야 한다. (reduce의 강점.)
  ex [1, 2, 3]
   1을 가지고 들어가서 [2, 3]의 요소와 합쳐준다. 그런 행위를 할때마다 push

```javascript
/**
 *  output : 배열
 * @param {*} inputArr : Input 배열이다. 
 * 전반적인 매커니즘은 pick에서 하나씩 가지고 재귀함수로 들어간다.
 */

function permutator(inputArr, rValue) {
  var results = [];
  var r = inputArr.length-rValue;
  
  function permute(arr, pick) {
    var cur, pick = pick || [];
    for (var i = 0; i < arr.length; i++) {
      cur = arr.splice(i, 1);
      results.push(pick.concat(cur));
      permute(arr.slice(), pick.concat(cur));
      arr.splice(i, 0, cur[0]);
    }
    return results;
  }

  return permute(inputArr);
}

```


# Permutation

```javascript
/**
* 만일 해당 조합을 숫자로 사용할 경우에 쓰도록 output이 String값.
*/
var myList = [1, 2, 3, 4];
var test = findAllPermutation(myList, 4); //4개 조합 ex 1234
function findPermutation(stringList, rValue) { 
    const len = stringList.length;
    const startCount = 1; //현재 조합을 뽑은 숫자. 재귀로 하나씩 뽑아갈때마다. count가 증가.

    function makePermutation(stringList, count, rValue, prevValue) {
        const frontPaddingValue = prevValue || ''; //값이 없을 경우 빈 문자로 초기화
        let pivotCount = count; //재귀로 들어올때마다 기준 Count를 정함.
        //reduce 과정에서 count값이 변경되므로 list의 요소가 끝날때 마다 다시 기준 Count를 잡아줌.
        return stringList.reduce( (listNumbers, value, idx) => {
            count = pivotCount;
            if(count === rValue) {
                listNumbers.push(frontPaddingValue+value);
                return listNumbers;
            }
            const nextNumberList = [...stringList];
            nextNumberList.splice(idx, 1);
            count++; //아래와 동일
            //count = 1+len-nextNumberList.length; 
            const result = makePermutation(nextNumberList, count, rValue, frontPaddingValue + value);
            listNumbers.push(...result);

            return listNumbers;
        }, []);
    }
    return makePermutation(stringList, startCount, rValue);
}
```
  findAllPermutation에 pivotCount를 지정하여 해당 Count일때의 집합을 저장.

```javascript
/**
 * 
 * @param {*} inputArr : Input 배열이다. 
 * @param {*} rValue : nPr에서 r이며, 배열의 길이보다 적게 들어와야한다.
 * 
 * 전반적인 매커니즘은 pick에서 하나씩 가지고 재귀함수로 들어간다.
 */

function permutator(inputArr, rValue) {
  var results = [];
  var r = inputArr.length-rValue; // 정확히는 arr의 length가 남는 개수에 따라 result값에 push를 한다. 즉 1개를 썼으면 3개가 남았으니까 1개 result.
  
  function permute(arr, pick) {
  
    var cur, pick = pick || [];

    for (var i = 0; i < arr.length; i++) {
      cur = arr.splice(i, 1);
      if (arr.length === r) {
        results.push(pick.concat(cur));
      }
      permute(arr.slice(), pick.concat(cur));
      arr.splice(i, 0, cur[0]);
    }

    return results;
  }

  return permute(inputArr);
}
```


# Combination

 ## 하나의 집합 내에서 조합
```javascript
const getCombinations = (arr, m) => {
    const combinations = [];
    const picked = [];

    function find(picked) {
        if (picked.length === m) {
            const rst = [];
            for (let i of picked) {
                rst.push(arr[i]);
            }
            combinations.push(rst);
            return;
        } else {
            let start = picked.length ? picked[picked.length - 1] + 1 : 0;
            for (let i = start; i < arr.length; i++) {
                picked.push(i);
                find(picked);
                picked.pop();
            }
        }
    }
    find(picked);
    return combinations;
}
```


 하나의 집합 중 같은 값이 있을 경우
  ([1, 2, 2, 3] 과 같은 숫자에서 combination을 뽑고자 할떄)
```javascript
const getCombinations = (arr, m) => {
  arr.sort(); //같은 값이 연속으로 들어오도록 강제
  const combinations = [];
  const picked = [];
  const used = new Array(arr.length).fill(false);
  
  function find(picked) {
    if (picked.length === m) {
      const rst = [];
      for (let i of picked) {
        rst.push(arr[i]);
      }
      combinations.push(rst);
      return;
    } else {
      let start = picked.length ? picked[picked.length-1] + 1 : 0;
      for (let i = start; i < arr.length; i++) {
        if (i === 0 || arr[i] !== arr[i-1] || used[i-1]) {
          picked.push(i);
          used[i] = 1;
          find(picked);
          picked.pop();
          used[i] = 0;
        }
      }
    }
  }
  find(picked);
  return combinations;
}
```

 ## 두개의 집합간 조합
  ### 독립된 두개의 집합
```javascript
   function getCombination(arr_a, arr_b) {
    var answer = 0;
    const combination = [];
    arr_a.forEach( a => {
        arr_b.forEach( b => {
            combination.push([a, b]);
        })
    })
    return combination;
    } 
```

  ### 종속적인 두개의 집합
   아래의 문제해결은 하나의 배열안에 2개배열을 넣을 경우에 해당하고, 
   만일 특정 조건으로 2개 배열을 구성할수 있다면 기존 하나의 배열에서의 조합 문제 해결방식을 따른다.(조합문제_1 참고)
  ```javascript
   // parentSet = [0, 1, 2, 3, 4]
    var arraySet = [[0, 2], [1, 2], [3, 4]];
  ```
  ```javascript

function getCombination(arrays) {
    var answer = {};
    function makeCombination(i = 0, selected = []) {
        if (!arrays[i]) {
            selected.sort(); //당연히 조합이기 때문에 순서가 다른 같은 값은 중복처리해줘야 한다.
            answer[selected.join('')] = true;
            return;
        }
        arrays[i].filter(e => !selected.includes(e)).forEach(e => {
            makeCombination(i + 1, selected.concat([e]));
        });
    }
    
    makeCombination();
    return answer;
}
/** 
순서가 같은 다른 값 
 ex) arraySet = [[0, 2], [1, 2], [3, 4], [3, 4]]; 일 경우
      [0, 1, 4, 3] , [0, 1, 3, 4] 
 를 튜플로서 처리하자면 아래와 같다.
*/

  function getCombination(arrays) {
    var answer = [];
    
    function makeCombination(i = 0, selected = []) {
        if (!arrays[i]) {
            answer.push(selected);
            return;
        }
        arrays[i].filter(e => !selected.includes(e)).forEach(e => {
            makeCombination(i + 1, selected.concat([e]));
        });
    }
    
    makeCombination();
    return answer;
}
```
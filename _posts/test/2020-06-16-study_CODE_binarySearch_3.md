---
layout: post
author: test
title:  "이분탐색 : 가사 검색"
description: "연습 문제"
categories: [ test ]
tags: [standard ]
---
category: 2020 KAKAO BLIND RECRUITMENT


# 문제 설명

 친구들로부터 천재 프로그래머로 불리는 프로도는 음악을 하는 친구로부터 자신이 좋아하는 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 궁금하니 프로그램으로 개발해 달라는 제안을 받았습니다.
 그 제안 사항 중, 키워드는 와일드카드 문자중 하나인 '?'가 포함된 패턴 형태의 문자열을 뜻합니다. 와일드카드 문자인 '?'는 글자 하나를 의미하며, 어떤 문자에도 매치된다고 가정합니다. 예를 들어 "fro??"는 "frodo", "front", "frost" 등에 매치되지만 "frame", "frozen"에는 매치되지 않습니다.

 가사에 사용된 모든 단어들이 담긴 배열 words와 찾고자 하는 키워드가 담긴 배열 queries가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 순서대로 배열에 담아 반환하도록 solution 함수를 완성해 주세요.

 - 가사 단어 제한사항
 words의 길이(가사 단어의 개수)는 2 이상 100,000 이하입니다.
 각 가사 단어의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
 전체 가사 단어 길이의 합은 2 이상 1,000,000 이하입니다.
 가사에 동일 단어가 여러 번 나올 경우 중복을 제거하고 words에는 하나로만 제공됩니다.
 각 가사 단어는 오직 알파벳 소문자로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
 
 - 검색 키워드 제한사항
 queries의 길이(검색 키워드 개수)는 2 이상 100,000 이하입니다.
 각 검색 키워드의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
 전체 검색 키워드 길이의 합은 2 이상 1,000,000 이하입니다.
 검색 키워드는 중복될 수도 있습니다.
 각 검색 키워드는 오직 알파벳 소문자와 와일드카드 문자인 '?' 로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
 검색 키워드는 와일드카드 문자인 '?'가 하나 이상 포함돼 있으며, '?'는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.
 예를 들어 "??odo", "fro??", "?????"는 가능한 키워드입니다.
 반면에 "frodo"('?'가 없음), "fr?do"('?'가 중간에 있음), "?ro??"('?'가 양쪽에 있음)는 불가능한 키워드입니다.
 
 - 입출력 예
| words | queries | result |
| ["frodo", "front", "frost", "frozen", "frame", "kakao"] | ["fro??", "????o", "fr???", "fro???", "pro?"] | [3, 2, 4, 1, 0] |

 
# 문제풀이

  정답률 0.8%에 빛나는 극악의 난이도 문제이다. 정확성 정답률은 34%이나 배점이 100점중 25점인게 함정.

  이진탐색을 타이틀로 걸었지만, 사실 이것보다 더 효율적으로 풀 수 있는 방법이 있다. 아래에서 설명하고겠다.
  극악의 난이도를 자랑 하는 이유는 단순히 이진 탐색에서 답을 찾는 것아 아닌 upper_bound와 lower_bound를 찾아야 하는 것이었다.
  그러나 그것 만으로 부족하다며 이진 탐색 compare 조건이 2중으로 만들어야 해결이 가능했다.

  (물론 해설에서는 앞서 언급한 이것보다 더 좋은 방법을 해설하였지만..)

  
  - 첫 시도 단순 선형 풀이 정확도 100%, 속도 40%
   일일히 찾는 방법. 아마도 대부분 이렇게 풀엇을 것으로 추측한다.

```javascript
function solution(words, queries) {
    var result = new Array(queries.length).fill(0);
    var querySet = makeQuerySet(queries);

    querySet.forEach((i, idx) => {
        words.forEach(j => {
            if (compare(j, i)) {
                result[idx]++;
            }
        })
    })

    return result;
}

function makeQuerySet(queries) {
    var querySet = [];
    
    queries.forEach( i => {
        var queryInfo = {};
        var len = i.length;
        queryInfo.length = len;
        if(i[0] === '?'){
            queryInfo.frontFlag = true;
            var num = markNum(i, true);
            queryInfo.mark = num;
            queryInfo.str = i.slice(num)
        } else {
            queryInfo.frontFlag = false;
            var num = markNum(i, false);
            queryInfo.mark = num;
            queryInfo.str = i.substring(0,len-num);
        }

        querySet.push(queryInfo);
    })
    return querySet;
}

function markNum(query, frontFlag) {
    let count = 1;
    var check = query.split('');
    if(frontFlag) {
        for(let i=1,len=check.length; i<len; i++) {
            if(check[i] === '?') {
                count++;
            } else {
                return count;
            }
        }
    } else {
        for(let j=check.length-2; j>=0; j--) {
            if(check[j] === '?') {
                count++;
            } else {
                return count;
            }
        }
    }
}

function compare(oneWord, oneQuery)  {
    if(oneWord.length !== oneQuery.length) {
        return false;
    }
    if(oneQuery.frontFlag) {
        if(frontCompare(oneWord, oneQuery)) {
            return true;
        } else {
            return false;
        }
    } else {
        if(backCompare(oneWord, oneQuery)) {
            return true;
        } else {
            return false;
        }
    }
}

function frontCompare(oneWord, oneQuery)  {
    return oneQuery.str === oneWord.slice(oneQuery.mark);
}

function backCompare(oneWord, oneQuery)  {
    return oneQuery.str === oneWord.substring(0,oneWord.length-oneQuery.mark);
}
```

 - 다음 여럿 시도 : querySet을 개수별로 다시 묶은 다음, 해당 범위 내에서 다시 찾기. 이진탐색으로 특정 값을 찾은 뒤 양옆으로 앞글자가 바뀔때까지 탐색 재개.

 - 문자열을 우선 길이 순으로 정렬한 뒤, 길이가 같다면 문자순으로 정렬을 하고, 해당 기준을 바탕으로 upper_bound 와 lower_bound를 찾는다. (단순한 bound와 다르니 찾아보자.)
   

```javascript
function solution(words, queries) {
  'use strict';
  var answer = [];

  var reverseWords = [];
  let wordStart = 0;
  let wordEnd = words.length-1;
  words.forEach(i => reverseWords.push(i.split("").reverse().join("")));

  let sortedWords = words.sort((a, b) => {
    if (a.length < b.length) {
      return a.length - b.length
    } else if (a.length === b.length) {
      return (a > b) - (a < b);
    } else {
      return a.length - b.length;
    }
  });
  

  let reverseSortedWords = reverseWords.sort((a, b) => {
    if (a.length < b.length) {
      return a.length - b.length
    } else if (a.length === b.length) {
      return (a > b) - (a < b);
    } else {
      return a.length - b.length;
    }
  });
  

  let len;
  let left;
  let right;

  queries.forEach(_ => {

    len = _.length;
    if (_[0] === '?') {
      var pivot = _.split("").reverse().join("");
      var leftPivot = pivot.split("").map((_, idx) => _ === '?' ? 'a' : _).join('');
      left = lowerBound(reverseSortedWords, wordStart, wordEnd, leftPivot);
      var rightPivot = pivot.split("").map((_, idx) => _ === '?' ? 'z' : _).join('');
      right = upperBound(reverseSortedWords, wordStart, wordEnd, rightPivot);

    } else {
     //debugger;
      var leftPivot = _.split("").map((_, idx) => _ === '?' ? 'a' : _).join('');
      left = lowerBound(sortedWords, wordStart, wordEnd, leftPivot);
      var rightPivot = _.split("").map((_, idx) => _ === '?' ? 'z' : _).join('');
      right = upperBound(sortedWords, wordStart, wordEnd, rightPivot);
    }

    answer.push(right - left);
  })

  return answer;
}

// ["frame", "frodo", "front", "frost", "kakao", "frozen"] 에서 ["fro???"] 를 lower는 5를 upper는 6을 찾는게 핵심이다.

function lowerBound(list, start, end, key) {
  let it;
  let step;
  let advance;
  let first = start;
  let count = end - start + 1;

  while (count > 0){
    step = Math.floor(count/2);
    advance = first+step;
    it = list[advance];
    if(compare(it,key) && it !== undefined) {
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
    if(!compare(key, it) && it !== undefined) {
      first = ++advance;
      count -= step+1;
    } else {
      count = step;
    }
  }
  return first;
}

function compare(a, b) {
  if(a.length<b.length) {
    return true;
  } else if(a.length === b.length) {
    if(a < b) {
      return true;
    }
  }
  return false;
}

```


 # 트라이 구조

  트라이 구조는 다음에 공부하자!
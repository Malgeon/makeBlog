---
layout: post
author: john
title:  "해시 : 완주하지 못한 선수"
categories: [ Study ]
---


# 문제 설명

 99한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.
 각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.
  

 입출력 예
 | n | result |
 | --- | --- |
 | "17" | 3 |
 | "011" | 2 |
 

# 문제풀이
  전체 순열 조합을 구하고, 그 중 primeNumber를 걸러낸다. 그 개수가 정답.
  
 
```javascript
function solution(numbers) {
    const numberList = numbers.split('');

    const findPrimeNumbers = (stringList, prevNumber) => {
        const caliPrevNumber = prevNumber || '';

        return stringList.reduce((list, value, idx) => {
            if (isPrimeNumber(Number(caliPrevNumber + value))) {
                list.push(Number(caliPrevNumber + value));
            }

            const nextList = [...stringList];
            nextList.splice(idx, 1);

            const result = findPrimeNumbers(
            nextList,
            caliPrevNumber + value,
            );
            list.push(...result);

            return list;
        }, []);
    }
    const answers = findPrimeNumbers(numberList);

    return [...new Set(answers)].length; //중복값 제거.
}



function isPrimeNumber(number) {
    const notPrime = [0, 1];
    if (notPrime.includes(number)) return false;
    for (let i = 2; i * i <= number; i++) {
        if (number % i === 0) return false;
    }
    return true;
}
```

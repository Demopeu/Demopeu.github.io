---
layout: single
title: "[Programmers] 43236/징검다리/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 이분탐색

image:
  path: https://Demopeu.github.io/images/logo/PROGRAMMERS.png
  alt: "PROGRAMMERS"
  thumbnail: true
toc: true
author_profile: false
sidebar:
  nav: "counts"
use_math: true
---

![PROGRAMMERS](https://Demopeu.github.io/images/logo/PROGRAMMERS.png)

# 💡 문제 설명

프로그래머스 문제 설명은 링크로 대체합니다.

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/43236)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| distance | rocks | n |
| -------- | ----- | - |
| 25       | [2, 14, 11, 21, 17] | 2 |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 4  |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 도착지점까지의 거리 distance는 1 이상 1,000,000,000 이하입니다.
- 바위는 1개 이상 50,000개 이하가 있습니다.
- n 은 1 이상 바위의 개수 이하입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
distance: 25;
rocks: [2, 14, 11, 21, 17];
n: 2;
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
4;
```

# 💡 풀이

## ✍️ 풀이과정

단순한 이분탐색 문제. 앞으로 mid 할때 비트 연산자로하면 될듯;

## 📖내가 작성한 JS Code

```javascript
function checkDistance(mid,n,array){
    let start = 0;
    let cnt = n;
        for(rock of array){
            if(rock-start < mid) cnt--
        else start = rock;
            if(cnt<0) return false;
    }
    return true;
}

function solution(distance, rocks, n) {
    const locations = [...rocks.sort((a,b)=>a-b),distance];
    let left = 0, right = distance;
    
    while (left<=right){
        const mid = (left+right) >> 1;
        if (checkDistance(mid,n,locations)) left = mid+1;
        else right = mid-1;
    }
    return right;
}
```

# 🧠 코드 리뷰

- **중간값 계산 안전화**: 비트 연산(`>> 1`) 대신 `Math.floor((left + right) / 2)` 사용. 32비트 강제 변환으로 인한 범위/정밀도 문제를 회피합니다.
- **변수 스코프 누수 방지**: `for(rock of array)`를 `for (const rock of array)`로 수정해 전역 변수 누수를 방지했습니다.
- **정렬과 종점 처리**: `rocks.slice().sort(...); locations.push(distance);` 형태로 불변 정렬 후 도착지점을 명시적으로 추가했습니다.
- **탐색 하한**: `left = 1`로 시작해 `mid = 0`에 대한 불필요 검사를 생략했습니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20251014js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/43236)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

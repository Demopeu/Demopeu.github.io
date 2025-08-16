---
layout: single
title: "[Programmers] 42578/의상/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 자료구조
  - 구현
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

프로그래머스라 문제 설명은 링크로 대체

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| clothes          |
| ---------------- |
| [["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]] |
| [["crow_mask", "face"], ["blue_sunglasses", "face"], ["smoky_makeup", "face"]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 5      |
| 3      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 코니가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
5
```

# 💡 풀이

## ✍️ 풀이과정

많은 풀이법이 있지만, reduce를 연습하고 싶었다. 소위, 멋쟁이?코드 같은 느낌임. 그냥 빠르게 풀었음.

## 📖내가 작성한 JS Code

```javascript
function solution(clothes) {
    return Object.values(clothes.reduce((acc,[,cur]) => ((acc[cur] = (acc[cur]||1)+1), acc), {})).reduce((acc,cur)=>acc*cur,1)-1;
}
```

# 🧠 코드 리뷰

## 요약
- __정확성__: 조합 공식 정확. 시간복잡도 O(n).
- __장점__: 간결한 one-liner.
- __개선__: 가독성 위해 집계와 곱셈 분리, 콤마 연산자 제거 권장.

### 가독성 버전
```javascript
function solution(clothes) {
  const counts = clothes.reduce((freq, [, type]) => {
    freq[type] = (freq[type] || 0) + 1; // 분류별 개수 집계
    return freq;
  }, {});
  return Object.values(counts).reduce((prod, c) => prod * (c + 1), 1) - 1; // (count+1) 곱 - 1
}
```

### 한 줄 설명
- 각 종류별 (입지 않음 포함) 경우의 수를 모두 곱하고, 전부 안 입은 1가지를 뺀다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250816js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

---
layout: single
title: "[Programmers] 42746/가장 큰 수/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 자료구조
  - 정렬
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42746)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| numbers                         |
| ---------------------------------- |
| [6, 10, 2]  |
| [3, 30, 34, 5, 9]              |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| "6210"  |
| "9534330"  |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[6, 10, 2]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
6102
```

# 💡 풀이

## ✍️ 풀이과정

js 좋은 점이 문자열도 크기 비교가 가능하다는 점이다. a+b와 b+a를 단순 마이너스 연산을 해도 된다. 그런데 혹여나 다른 문자가 들어오면 NaN이 나오니까 안전하게 부등호 쓰긴함.

## 📖내가 작성한 JS Code

```javascript
function solution(numbers) {
    return numbers.reduce((acc,cur)=>acc+cur,0) === 0? '0': numbers.map(String).sort((a,b)=>(b+a > a+b? 1:-1)).join('')
}
```

# 🧠 코드 리뷰

## 요약
- __정확성__: 정렬 기준 `(b+a)` vs `(a+b)` 사용으로 정답. 전부 0 처리도 OK.
- __복잡도__: O(n log n · k). k는 자릿수(≤4)로 상수 취급.
- __개선__: 비교 함수 가독성, 0 처리 방식(정렬 후 첫 원소 체크) 권장.

### 가독성 버전
```javascript
function solution(numbers) {
  const arr = numbers.map(String).sort((a, b) => (b + a).localeCompare(a + b));
  return arr[0] === '0' ? '0' : arr.join('');
}
```

### 코멘트
- 정렬 비교는 `localeCompare`로 -1/0/1을 명확히 반환해 의도가 드러납니다.
- 모든 원소가 0인 경우는 정렬 후 첫 원소가 '0'인지로 처리하면 한 번에 해결됩니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250816js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42746)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
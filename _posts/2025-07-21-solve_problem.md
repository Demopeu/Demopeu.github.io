---
layout: single
title: "[Baekjoon] 23293/아주 서바이벌/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 구현
image:
  path: https://Demopeu.github.io/images/logo/BAEKJOON.png
  alt: "BAEKJOON"
  thumbnail: true
toc: true
author_profile: false
sidebar:
  nav: "counts"
use_math: true
---

![BAEKJOON](https://Demopeu.github.io/images/logo/BAEKJOON.png)

# 💡 문제 설명

위대한 해커 창영이는 모든 암호를 깨는 방법을 발견했다. 그 방법은 빈도를 조사하는 것이다.

창영이는 말할 수 없는 방법을 이용해서 현우가 강산이에게 보내는 메시지를 획득했다. 이 메시지는 숫자 N개로 이루어진 수열이고, 숫자는 모두 C보다 작거나 같다. 창영이는 이 숫자를 자주 등장하는 빈도순대로 정렬하려고 한다.

만약, 수열의 두 수 X와 Y가 있을 때, X가 Y보다 수열에서 많이 등장하는 경우에는 X가 Y보다 앞에 있어야 한다. 만약, 등장하는 횟수가 같다면, 먼저 나온 것이 앞에 있어야 한다.

이렇게 정렬하는 방법을 빈도 정렬이라고 한다.

수열이 주어졌을 때, 빈도 정렬을 하는 프로그램을 작성하시오.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 메시지의 길이 N과 C가 주어진다. (1 ≤ N ≤ 1,000, 1 ≤ C ≤ 1,000,000,000)

둘째 줄에 메시지 수열이 주어진다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫째 줄에 입력으로 주어진 수열을 빈도 정렬한 다음 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
5 2
2 1 2 1 2
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
2 2 2 1 1
```

# 💡 풀이

## ✍️ 풀이과정

현대 오토에버가 돌아오기 때문에 다시 시작한 JavaScript 재활 훈련. Map을 사용해야하나? 라는 생각이 들어서 약간 매몰됨. {}가 JavaScript에서 있었는지 기억이 나지 않았음.

1. input 받기 오랜만에 해서 최적화 어려웠음.
2. 자료구조 선택의 어려움(js에 이게 있엇나? 라는 어려움)
3. sort() 함수 사용법 햇갈림

요즘 input은 그냥 주는 함수형(프로그래머스)가 대세기 때문에 1번은 그려려니 하는데, js 자료구조는 오늘 다시 확인해야할듯. 그리고 sort(),join()은 좀 손에 익혀야 할듯.

## 📖 내가 작성한 JS Code

```javascript
const fs = require("fs");
const input = fs.readFileSync("./input.txt").toString().trim().split("\n");

const solution = (input) => {
  const dictionary = new Map();
  const order = [];

  for (num of input[1].split(" ")) {
    if (!dictionary.has(num)) {
      order.push(num);
      dictionary.set(num, 0);
    }
    dictionary.set(num, dictionary.get(num) + 1);
  }

  order.sort(function (a, b) {
    return dictionary.get(b) - dictionary.get(a);
  });

  const answer = [];
  for (idx of order) {
    for (let i = 0; i < dictionary.get(idx); i++) {
      answer.push(idx);
    }
  }
  return answer.join(" ");
};

console.log(solution(input));

```

---

# 🧠 코드 리뷰

## 1. 개선 포인트: 동일 빈도 숫자 처리 로직 명시

**문제점:**
현재 정렬 로직은 숫자의 빈도수만 고려하고 있습니다. 문제의 조건에 따르면, 만약 두 숫자의 빈도수가 같다면, 원래 수열에서 먼저 나타난 숫자가 정렬 후에도 앞에 위치해야 합니다. 사용하신 `order.sort()`는 이 '동일 빈도수' 경우의 처리 로직이 명시적으로 없어, JavaScript 엔진의 `sort` 구현 안정성(stability)에 따라 결과가 달라질 수 있습니다. 코드의 의도를 명확히 하고 어떤 환경에서든 일관된 결과를 보장하기 위해 동일 순위 처리 로직을 명시적으로 추가하는 것이 좋습니다.

**기존 코드:**
```javascript
  order.sort(function (a, b) {
    return dictionary.get(b) - dictionary.get(a);
  });
```

**개선 방향:**
빈도수가 동일할 경우, 숫자가 처음 등장한 순서를 비교하는 로직을 추가합니다. `order` 배열은 이미 첫 등장 순서대로 숫자를 담고 있으므로, 이 순서를 정렬의 2차 기준으로 활용할 수 있습니다. 정렬 과정에서 배열이 변경되므로, 정렬 전의 순서를 별도 변수에 저장해두고 비교 기준으로 사용합니다.

**개선된 코드:**
```javascript
  const originalOrder = [...order]; // 정렬 전, 첫 등장 순서를 보존
  order.sort(function (a, b) {
    // 1. 빈도수를 기준으로 내림차순 정렬
    const freqDiff = dictionary.get(b) - dictionary.get(a);
    if (freqDiff !== 0) {
      return freqDiff;
    }
    // 2. 빈도수가 같으면, 첫 등장 순서(오름차순)로 정렬
    return originalOrder.indexOf(a) - originalOrder.indexOf(b);
  });
```

---

## 2. 개선 포인트: 변수명 가독성 향상

**문제점:**
`dictionary`와 `order`라는 변수명은 범용적이라 해당 변수가 코드 내에서 어떤 데이터와 역할을 담고 있는지 즉시 파악하기 조금 아쉽습니다.

**기존 코드:**
```javascript
  const dictionary = new Map();
  const order = [];
```

**개선 방향:**
변수의 역할을 명확하게 설명하는 이름을 사용하면 코드의 가독성을 크게 높일 수 있습니다. 예를 들어, `dictionary`는 `frequencies` (빈도) 또는 `counts`로, `order`는 `appearanceOrder` (등장 순서) 또는 `uniqueNumbers` 등으로 변경하면 좋습니다.

**개선된 코드:**
```javascript
  const frequencies = new Map(); // 'dictionary' -> 'frequencies'
  const appearanceOrder = [];    // 'order' -> 'appearanceOrder'
```
(코드 전체에서 변수명을 일관되게 수정해야 합니다.)

---

## 3. 개선 포인트: 결과 배열 생성 로직 간소화

**문제점:**
현재 최종 결과 문자열을 만들기 위해, `answer` 배열을 선언하고 중첩 `for` 루프를 통해 요소를 하나씩 `push`하고 있습니다. 이는 잘 동작하는 코드이지만, 최신 JavaScript의 배열 메소드를 사용하면 더 간결하고 선언적으로 코드를 작성할 수 있습니다.

**기존 코드:**
```javascript
  const answer = [];
  for (idx of order) {
    for (let i = 0; i < dictionary.get(idx); i++) {
      answer.push(idx);
    }
  }
  return answer.join(" ");
```

**개선 방향:**
`flatMap`과 같은 고차 함수를 사용하면 중첩 루프 없이 결과 배열을 효율적으로 생성할 수 있습니다. `flatMap`은 각 요소에 대해 배열을 반환하고, 그 결과들을 모두 펼쳐서 새로운 단일 배열로 만들어줍니다.

**개선된 코드:**
```javascript
  const answer = order.flatMap(num =>
    Array(frequencies.get(num)).fill(num)
  );
  return answer.join(" ");
```
(여기서는 개선된 변수명 `frequencies`를 사용했습니다.)

---

전반적으로 문제의 요구사항을 잘 이해하고 Map을 활용하여 효율적으로 풀이하신 점이 좋습니다! 위에 제안된 내용들을 반영하시면 코드가 더욱 견고하고 가독성이 높아질 것입니다.

---

# 💻결과

<strong style="font-size: 1.2em">🐍 python</strong>

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-07-21/j_result.PNG)

[백준문제 보러가기](https://www.acmicpc.net/problem/23293)

---

# 🖱️참고 링크

[MDN web docs js Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)

[MDN web docs js sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

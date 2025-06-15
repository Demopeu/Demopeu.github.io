---
layout: single
title: "[Baekjoon] 25972/도미노 무너트리기/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 그리디 알고리즘
  - 정렬
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

미야노는 N개의 도미노를 가지고 놀고 있다. 각각의 도미노는 1차원 좌표계의 x좌표 위에 위치하고 있고 길이를 가진다. i번째 도미노의 x좌표를 a_i, 길이를 l_i라 하자. 도미노는 오른쪽으로 무너트릴 수 있다. 길이 l_i를 가지는 도미노가 위치 a_i에 있을 때 오른쪽으로 무너질 경우 좌표 값이 a_i보다 크고 a_i+l_i보다 작거나 같은 도미노 중 가장 작은 좌표를 가지는 도미노가 오른쪽으로 무너진다.

미야노는 도미노를 최소한의 횟수로 무너트려서 모든 도미노를 무너트리려고 한다. 머리가 나쁜 미야노는 최소한의 횟수를 구하지 못해 여러분에게 답을 물어봤다. 미야노를 위해 모든 도미노가 무너지려면 처음에 몇 개의 도미노를 무너트려야 하는지 구해주자.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫 번째 줄에 N이 주어진다.
$(1 ≤ N ≤ 500\,000)$

두 번째 줄부터 N+1번째 줄 까지 a_i, l_i가 공백으로 구분되어 주어진다.
$(1 ≤ a_i ≤ 10^9,1 ≤ l_i ≤ 10^9)$

어떤 두 도미노가 같은 x좌표를 가지는 경우는 주어지지 않는다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

모든 도미노가 무너지기 위해 미야노가 처음에 무너트려야 할 도미노의 갯수를 구해주자.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
1
100 10
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
1
```

# 💡 풀이

## ✍️ 풀이과정

이런 문제에 시간을 낭비하다니 열받음. 분명 도미노라 최대 길이를 max로 갱신했는데 이거 무조건 도미노 1개씩만 넘어짐. 진짜 스트레스다. 문제를 잘못 만든듯. 예제도 sort 되어있는거처럼 되어 있음.

## 📖 내가 작성한 Python Code

```python

```

## 📖내가 작성한 JS Code

```javascript
const fs = require("fs");
const inputs = fs.readFileSync(0, "utf-8").toString().trim().split(/\s+/);

const problem = (inputs) => {
  const N = Number(inputs[0]);
  const dominos = [];
  for (let i = 0; i < N; i++) {
    const x = +inputs[1 + 2 * i];
    const l = +inputs[1 + 2 * i + 1];
    dominos.push({ x, l });
  }

  dominos.sort((a, b) => a.x - b.x);

  let max_length = 0;
  let count = 0;

  for (let { x, l } of dominos) {
    if (max_length < x) count++;
    max_length = x + l;
  }
  return count;
};

console.log(problem(inputs));
```

---

# 🧠 코드 리뷰

큰 문제는 없습니다.

---

# 💻결과

<strong style="font-size: 1.2em">🐍 python</strong>

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-06-15/j_result.PNG)

[백준문제 보러가기](https://www.acmicpc.net/problem/25972)

---

# 🖱️참고 링크

[나무위키 그리디 알고리즘 참고](https://namu.wiki/w/%EA%B7%B8%EB%A6%AC%EB%94%94%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

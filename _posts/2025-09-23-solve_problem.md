---
layout: single
title: "[백준] 17225/세훈이의 선물가게/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 시뮬레이션
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

세훈이는 선물가게를 운영한다. 세훈이의 선물가게는 특이하게도 손님이 어떤 선물을 구매할지 선택할 수가 없다. 대신 세훈이의 취향으로 랜덤하게 준비된 선물 중 몇 개를 구매할 것인지, 파란색과 빨간색 중 어떤 색으로 포장 받을 것인지만 결정해 주문할 수 있다.

상민이와 지수는 세훈이의 가게에서 선물 포장을 맡은 아르바이트생이다. 손님들은 파란색 포장지를 원하면 상민이에게, 빨간색 포장지를 원하면 지수에게 주문을 한다. 두 사람은 각자 주문을 받으면 그때부터 포장을 시작하는데, 현재 남아있는 선물 중 가장 앞에 있는 선물을 가져와 포장하고 주문을 받은 개수만큼 이를 반복하는 형태다. 이때 선물 하나를 포장하는 데 상민이는 A초, 지수는 B초가 걸린다. 두 사람 모두 받거나 밀린 주문이 없는데 미리 선물을 가져오거나 포장하는 일은 없으며, 두 사람이 동시에 선물을 가져올 때는 알바짬이 조금 더 있는 상민이가 먼저 가져오고, 지수가 그 뒤의 선물을 가져온다.

세훈이는 어제 구매한 선물이 망가져 있다는 항의 전화를 받았다. 자신이 준비한 선물에는 문제가 없었기에 손님에게 포장지의 색을 물었지만, 손님은 자신이 받은 선물이 무엇인지만 말하며 화를 낼 뿐이었다. 어쩔 수 없이 세훈이는 어제 가게를 방문한 손님들의 주문 내역을 보고 그 선물을 누가 포장했는지 파악하려 한다.

방문한 손님의 수와 각 손님이 주문한 시각, 선택한 포장지, 포장 받을 선물의 개수가 주어졌을 때 상민이와 지수가 각자 어떤 선물들을 포장했는지 알아내는 프로그램을 작성해보자.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫 줄에 상민이가 선물 하나를 포장하는 데 걸리는 시간 A, 지수가 선물 하나를 포장하는 데 걸리는 시간 B, 어제 세훈이 가게의 손님 수 N(1 ≤ N ≤ 1,000)이 주어진다.

이후 N개의 줄에 걸쳐 1번부터 N번 손님의 주문 시각 ti(1 ≤ ti ≤ 86,400), 선택한 포장지의 색깔 ci(ci = "B"|"R"), 주문한 선물의 개수 mi(1 ≤ mi ≤ 100)가 주어진다.

ti는 가게가 오픈한 지 ti초 후에 손님이 주문했음을 뜻하며 ci는 포장지의 색깔을 의미하는 알파벳으로 "B"는 파란색을, "R"은 빨간색을 의미한다. 주어지는 입력은 시간의 흐름에 맞게 ti의 오름차순으로 주어지며, 서로 같은 시간에 주문한 손님은 없다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫 번째 줄에 상민이가 포장한 선물의 개수를 출력한다. 이후 두 번째 줄에 상민이가 포장한 선물들의 번호를 오름차순으로 공백으로 구분하여 출력한다.

세 번째 줄에 지수가 포장한 선물의 개수를 출력한다. 이후 네 번째 줄에 지수가 포장한 선물들의 번호를 오름차순으로 공백으로 구분하여 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- A = B = 0

  - A, B가 0이라는 것은 해당 아르바이트생의 포장 속도가 너무 빨라서, 주문과 동시에 해당 주문의 모든 선물 포장이 끝난다는 의미이다.

- 0 ≤ A, B ≤ 300

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
0 0 3
1 B 3
4 R 2
7 R 2
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
3
1 2 3
4
4 5 6 7
```

# 💡 풀이

## ✍️ 풀이과정

시뮬 열심히 연습 중인데, 삼성 서탈함. 허허. 골드 5까지는 그냥 구현이라서 쉬운듯 내일부터는 난이도를 올릴듯.

## 📖내가 작성한 JS Code

```javascript
/*
1. 요약:
  - 랜덤 선물 사기 * 빨파 중 선택
  - 파란색 상민
  - 빨간색 지수
  - 가장 앞에 있는 선물 포장하고 주문 받은 만큼 반복
  - 상민 A초
  - 지수 B초
  - 동시에는 상민이 first
  - 선물 망가진거 찾기
2. 아이디어:
  - 선물의 총개수를 일단 구함
  - array를 하나씩 빼서 큐에 쌓아둬야할듯?
  - while해서 상민큐,지수큐,주문큐 없으면 끝?
  - time 1씩해서 늘리면 될듯?
3. 시간복잡도:
  - 시간 : 86,400 * 300 * 100 대충 1초 될듯?

*/

const fs = require("fs");
const input = fs.readFileSync(0).toString().trim().split(/\s+/);

const [a, b, n] = input.slice(0, 3).map(Number);
const array = Array.from({ length: n }, (_, idx) =>
  input.slice(3 + idx * 3, 3 + idx * 3 + 3)
);

function solution(a, b, n, array) {
  let time = 0;
  let sangminCount = 0;
  let jisuCount = 0;
  let presentCount = 1;
  const sangminQueue = [];
  const jisuQueue = [];
  const presentsQueue = array;
  const sangmin = [];
  const jisu = [];

  while (sangminQueue.length || jisuQueue.length || presentsQueue.length) {
    time++;
    if (presentsQueue.length && Number(presentsQueue[0][0]) === time) {
      const person = presentsQueue.shift();
      person[1] === "B"
        ? (sangminCount += Number(person[2]))
        : (jisuCount += Number(person[2]));
    }

    while (true) {
      if (!sangminQueue.length) {
        if (sangminCount === 0) break;
        sangminCount--;
        sangminQueue.push([a, presentCount++]);
      } else {
        sangminQueue[0][0]--;
      }
      if (sangminQueue[0][0] !== 0) {
        break;
      } else {
        const node = sangminQueue.shift();
        sangmin.push(node[1]);
      }
    }

    while (true) {
      if (!jisuQueue.length) {
        if (jisuCount === 0) break;
        jisuCount--;
        jisuQueue.push([b, presentCount++]);
      } else {
        jisuQueue[0][0]--;
      }
      if (jisuQueue[0][0] !== 0) {
        break;
      } else {
        const node = jisuQueue.shift();
        jisu.push(node[1]);
      }
    }
  }
  console.log(sangmin.length);
  console.log(...sangmin);
  console.log(jisu.length);
  console.log(...jisu);
}

solution(a, b, n, array);
```

# 🧠 코드 리뷰

# 요약

- 입력 파싱과 시뮬레이션 루프 구조가 문제 요구사항을 잘 반영했습니다. `presentCount`로 전체 선물 번호를 일관되게 증가시키는 방식이 깔끔합니다.

# 개선 제안

- 동시 처리 규칙(동시에 선물을 가져올 때 상민 우선)이 현재 로직에서는 각자의 while 블록이 독립적으로 돌기 때문에 미묘한 경합 상황에서 의도치 않은 순서를 만들 수 있습니다. 동일 시각에 두 큐가 비어 있고 두 사람 모두 새 작업을 시작하는 경우, 하나의 단계에서 상민을 먼저 확정하고 그 다음 지수를 확정하도록 “한 틱 내 결정 순서”를 명시적으로 분리하면 안전합니다.
- `A=0` 또는 `B=0`인 즉시 포장 케이스에서 대량의 작업이 한 틱에 몰릴 수 있으므로, 해당 경우는 큐에 누적 push 없이 즉시 결과 배열에 넣는 최적화 분기를 추가하면 시간 경과 루프를 더 줄일 수 있습니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250923js.png)

[백준 문제 보러가기](https://www.acmicpc.net/problem/17225)

# 🖱️참고 링크

[MDN- slice](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

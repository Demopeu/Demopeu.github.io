---
layout: single
title: "[백준] 1021/회전하는 큐/javascript"
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

지민이는 N개의 원소를 포함하고 있는 양방향 순환 큐를 가지고 있다. 지민이는 이 큐에서 몇 개의 원소를 뽑아내려고 한다.

지민이는 이 큐에서 다음과 같은 3가지 연산을 수행할 수 있다.

1. 첫 번째 원소를 뽑아낸다. 이 연산을 수행하면, 원래 큐의 원소가 a1, ..., ak이었던 것이 a2, ..., ak와 같이 된다.
2. 왼쪽으로 한 칸 이동시킨다. 이 연산을 수행하면, a1, ..., ak가 a2, ..., ak, a1이 된다.
3. 오른쪽으로 한 칸 이동시킨다. 이 연산을 수행하면, a1, ..., ak가 ak, a1, ..., ak-1이 된다.

큐에 처음에 포함되어 있던 수 N이 주어진다. 그리고 지민이가 뽑아내려고 하는 원소의 위치가 주어진다. (이 위치는 가장 처음 큐에서의 위치이다.) 이때, 그 원소를 주어진 순서대로 뽑아내는데 드는 2번, 3번 연산의 최솟값을 출력하는 프로그램을 작성하시오.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 큐의 크기 N과 뽑아내려고 하는 수의 개수 M이 주어진다. N은 50보다 작거나 같은 자연수이고, M은 N보다 작거나 같은 자연수이다. 둘째 줄에는 지민이가 뽑아내려고 하는 수의 위치가 순서대로 주어진다. 위치는 1보다 크거나 같고, N보다 작거나 같은 자연수이다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫째 줄에 문제의 정답을 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
10 3
1 2 3
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
0
```

# 💡 풀이

## ✍️ 풀이과정

시뮬레이션문제로 인덱스로 구현해봄.

## 📖내가 작성한 JS Code

```javascript
/*
  1. 아이디어 : 시뮬레이션 문제, 하라는 데로 해보자.
  - 첫 원소 뽑기
  - 왼 포인터 이동
  - 오른 포인터 이동
  - 큐의 크기 N?
  - 뽑는 개수 M
  - 그 원소를 주어진 순서대로 뽑아내는데 드는 2,3번 연산의 최솟값?

  2. 시간복잡도 : 왼오른만 구분하면 될듯.
  3. 자료구조: 그냥 array로 계속 돌아보자.
  */

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

const [n, _, ...array] = input;

function solution(n, array) {
  let [answer, idx] = [0, 0];
  const queue = Array.from({ length: n }, (_, idx) => idx + 1);
  for (const node of array) {
    const target = queue.indexOf(node);
    const queueLength = queue.length;
    const left = (target - idx + queueLength) % queueLength;
    const right = queueLength - left;

    if (left <= right) {
      answer += left;
      idx = (idx + left) % queueLength;
    } else {
      answer += right;
      idx = (idx - right + queueLength) % queueLength;
    }
    queue.splice(idx, 1);
    idx %= queueLength - 1;
  }
  return answer.toString();
}

process.stdout.write(solution(n, array));
```

# 🧠 코드 리뷰

- **접근 방식**: `idx`로 현재 큐의 머리를 추적하고, 목표 원소까지의 왼쪽/오른쪽 이동 비용을 각각 `left`, `right`로 계산해 더 작은 쪽을 선택하는 전형적인 시뮬레이션 풀이입니다. 실제 배열을 회전시키지 않고 인덱스만 이동시킨 후 `splice`로 제거하는 방식이라 구현이 깔끔합니다.
- **정확성**: `left = (target - idx + queueLength) % queueLength`, `right = queueLength - left` 계산이 올바르며, 동률(`left <= right`)일 때 왼쪽 회전을 선택해도 최소 이동 횟수 보장이 됩니다. 제거 후 `idx`를 유지한 채 다음 상태로 넘어가는 것도 일관성 있습니다.
- **시간/공간 복잡도**: 각 단계에서 `indexOf`와 `splice`가 O(N)이라 최악 O(N^2)입니다. 본 문제 제약(N ≤ 50)에서는 충분히 빠릅니다. 추가 공간은 O(N).
- **경계값/주의사항**:
  - 마지막 원소를 제거한 직후 `idx %= queueLength - 1`에서 분모가 0이 될 수 있습니다. 현재 로직상 다음 반복에서 `idx`를 사용하지 않아도 NaN을 대입하는 것은 바람직하지 않습니다. 안전하게 길이가 0이 아닐 때만 모듈러를 적용하세요.
  - 백준 입출력은 보통 `/dev/stdin`을 사용합니다. 로컬 테스트 파일 경로(`./input.txt`)는 제출 시 실패 원인이 될 수 있습니다.
- **개선 제안 (소폭 리팩터링)**:
  - I/O: `fs.readFileSync(0, 'utf8')` 또는 `fs.readFileSync('/dev/stdin', 'utf8')`로 교체.
  - 인덱스 보정: 제거 후 `if (queue.length) idx %= queue.length;`로 가드 처리.
  - 가독성: `M`(뽑는 개수)도 구조분해 시 변수명으로 받거나 주석에 명시하면 의도가 더 분명해집니다.
- **대안 접근**: 덱 구현으로 실제 회전을 수행하거나, 머리 포인터만 이동하는 현재 방식처럼 “가상 회전”을 유지하되 `shift/unshift`를 피하는 것이 성능상 유리합니다. 현재 해법이 실용적으로 가장 간결합니다.
- **총평**: 제약에 맞춘 간결하고 정확한 시뮬레이션 풀이입니다. 입출력 경로와 마지막 단계의 `idx` 보정만 보강하면 제출/재사용 모두에 안전합니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250919js.png)

[백준 문제 보러가기](https://www.acmicpc.net/problem/1021)

# 🖱️참고 링크

[MDN- indexOf](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

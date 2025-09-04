---
layout: single
title: "[Programmers] 42860/조이스틱/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 그리디알고리즘

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42860)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| name     |
| -------- |
| "JEROEN" |
| "JAN"    |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 56     |
| 23     |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
name: "JEROEN";
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
56;
```

# 💡 풀이

## ✍️ 풀이과정

## 📖내가 작성한 JS Code

```javascript
function solution(name) {
  const upDownCount = name
    .split("")
    .reduce(
      (acc, cur) =>
        acc +
        Math.min(
          "Z".charCodeAt(0) - cur.charCodeAt(0) + 1,
          cur.charCodeAt(0) - "A".charCodeAt(0)
        ),
      0
    );

  let leftRightCount = name.length - 1;
  name.split("").forEach((e, idx, arr) => {
    let next = idx + 1;
    while (next < arr.length && arr[next] === "A") next++;
    leftRightCount = Math.min(
      leftRightCount,
      idx * 2 + arr.length - next,
      (name.length - next) * 2 + idx
    );
  });

  return upDownCount + leftRightCount;
}
```

# 🧠 코드 리뷰

* __상하 이동 계산(정확성)__: `min('Z' - cur + 1, cur - 'A')`로 각 문자 변경 최소 조작 수를 더하는 방식이 정답입니다. 'A'와의 거리 vs 'Z'에서 역방향 이동(+1 포함)이 정확히 반영되어 있습니다.
* __좌우 이동 계산(핵심 아이디어)__: 기본 이동을 `name.length - 1`로 두고, 연속되는 'A' 구간을 스킵하며 세 가지 경로를 비교합니다.
  - 오른쪽 진행 후 되돌아가기: `idx * 2 + (len - next)`
  - 왼쪽 먼저 갔다가 오른쪽: `(len - next) * 2 + idx`
  - 아무 최적화 없이 직진: `leftRightCount`(초기값 기준)
  이 조합이 조이스틱 최소 좌우 이동의 대표적인 그리디 패턴으로, 'A' 블록을 효율적으로 건너뜁니다.
* __경계값 처리__: 모든 문자가 'A'인 경우 `upDownCount = 0`, 'A' 블록 스킵으로 `leftRightCount`가 0으로 갱신되어 최종 0을 반환합니다. 길이 1, 혼합(음수 없음), 연속 'A' 양끝/중앙 배치 케이스에서도 올바르게 동작합니다.
* __시간/공간 복잡도__: O(N) 2패스(splitting + forEach). 추가 공간 O(N) (split 배열)이나 실제 상수 수준입니다. 충분히 효율적입니다.
* __가독성 제안(선택)__:
  - 문자 코드 상수를 도입해 의도를 명확히: `const A = 65, Z = 90` 또는 `'A'.charCodeAt(0)` 캐싱.
  - `split("")` 반복 호출을 줄여 한 번만 분해하거나, 전통 `for` 루프로 인덱스·문자 접근을 통합.
  - 변수 명을 문제 도메인에 맞게: `leftRightCount` → `horizontalMoves`, `upDownCount` → `verticalMoves` 등.

### 리팩터링 예시(동일 로직, 가독성 강화)

```javascript
function solution(name) {
  const A = 'A'.charCodeAt(0);
  const Z = 'Z'.charCodeAt(0);
  const chars = [...name];

  let verticalMoves = 0;
  for (const ch of chars) {
    const code = ch.charCodeAt(0);
    verticalMoves += Math.min(Z - code + 1, code - A);
  }

  let horizontalMoves = name.length - 1;
  for (let i = 0; i < chars.length; i++) {
    let next = i + 1;
    while (next < chars.length && chars[next] === 'A') next++;
    horizontalMoves = Math.min(
      horizontalMoves,
      i * 2 + (chars.length - next),
      (chars.length - next) * 2 + i
    );
  }

  return verticalMoves + horizontalMoves;
}
```

* __테스트 팁__: 전부 'A', 'A'가 앞/중앙/뒤에 몰린 케이스, 교차 배치(`ABAAAAABA`), 길이 1, 최대 길이(20) 등으로 비교 검증을 권장합니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250904js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42884)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

<br>
[MDN- forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

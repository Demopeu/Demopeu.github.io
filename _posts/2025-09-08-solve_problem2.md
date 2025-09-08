---
layout: single
title: "[Programmers] 42897/도둑질/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 동적계획법

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42897)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| money     |
| --------- |
| [1,2,3,1] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 4      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 이 마을에 있는 집은 3개 이상 1,000,000개 이하입니다.
- money 배열의 각 원소는 0 이상 1,000 이하인 정수입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
money: [1, 2, 3, 1];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
4;
```

# 💡 풀이

## ✍️ 풀이과정

그냥 dp인데 처음 집 털까 안털까가 끝인 문제. 동그란 배치보면 바로 알 수 있음.

## 📖내가 작성한 JS Code

```javascript
function solution(money) {
  const len = money.length;
  const dp1 = Array(len).fill(0);
  const dp2 = Array(len).fill(0);
  dp1[0] = dp1[1] = money[0];
  dp2[1] = money[1];
  for (let i = 2; i < len; i++)
    dp1[i] = Math.max(dp1[i - 2] + money[i], dp1[i - 1]);
  for (let i = 2; i < len; i++)
    dp2[i] = Math.max(dp2[i - 2] + money[i], dp2[i - 1]);
  return Math.max(dp1[len - 2], dp2[len - 1]);
}
```

# 🧠 코드 리뷰

- 장점
  - 원형 도둑질 문제의 핵심을 정확히 짚었습니다. 첫 집을 털었다고 가정한 경우(`dp1`)와 털지 않은 경우(`dp2`)로 분리하여 선형 하우스 로버(House Robber)로 환원했습니다.
  - 초기값 설정이 타당합니다. `dp1[1] = money[0]`로 두어 첫 집을 선택한 경우 2번째 집은 선택 불가를 명확히 반영했습니다.
  - 두 번의 동일 패턴 루프(2..len-1)로 직관적인 점화식 `dp[i] = max(dp[i-1], dp[i-2] + money[i])`을 구현했습니다.

- 개선이 필요한 부분/권장 리팩터링
  1) 공간 복잡도: 현재 O(n) 배열 2개를 사용합니다. 롤링 변수(2칸 메모리)로 O(1)로 줄일 수 있습니다.
  2) 변수/네이밍 가독성: `dp1`, `dp2` 대신 `includeFirst`, `excludeFirst` 등 의미가 드러나는 이름을 권장합니다.
  3) 경계 처리 주석: 문제 제약상 집의 개수는 3 이상이지만, 일반성을 높이려면 `len <= 2`에 대한 설명/가드 주석을 더하면 독자 친화적입니다.

- 정확성
  - 원형 제약(첫 집과 마지막 집 동시 선택 불가)을 두 케이스로 분리하여 해결하는 접근은 정당합니다. 반환 시 `max(dp1[len-2], dp2[len-1])`로 마지막 집 포함/배제를 올바르게 반영합니다.

- 복잡도
  - 시간: O(n)
  - 공간: O(n) (배열 2개). 롤링 변수로 O(1) 가능

- 엣지 케이스 체크리스트
  - 전부 0인 입력: 결과는 0이어야 함
  - 매우 큰 n(최대 1,000,000): 배열 2개 생성의 메모리 사용량 고려 필요 → O(1) 공간 대안 권장
  - 큰 합계(최대 약 1e9): JS Number(53-bit 정밀) 범위 내에서 안전

- 리팩터링 예시(O(1) 공간, 동일 정답)

```javascript
function solution(money) {
  const n = money.length;
  if (n === 1) return money[0];
  if (n === 2) return Math.max(money[0], money[1]);

  // 케이스 1: 첫 집 포함(마지막 집 제외, 인덱스 0..n-2)
  let prev2 = money[0];           // dp[i-2]
  let prev1 = money[0];           // dp[i-1] (i=1일 때 money[0])
  for (let i = 2; i < n - 1; i++) {
    const cur = Math.max(prev1, prev2 + money[i]);
    prev2 = prev1;
    prev1 = cur;
  }
  const includeFirst = prev1;

  // 케이스 2: 첫 집 제외(마지막 집 포함 가능, 인덱스 1..n-1)
  prev2 = 0;                      // dp[0] = 0 (가상의 시작)
  prev1 = money[1];               // dp[1]
  for (let i = 2; i < n; i++) {
    const cur = Math.max(prev1, prev2 + money[i]);
    prev2 = prev1;
    prev1 = cur;
  }
  const excludeFirst = prev1;

  return Math.max(includeFirst, excludeFirst);
}
```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250908js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42895)

# 🖱️참고 링크

[MDN- Math.max](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

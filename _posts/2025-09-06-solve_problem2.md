---
layout: single
title: "[Programmers] 42895/N으로 표현/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42895)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| N | numbers     |
| -------- |
| 5 | 12 |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 4      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
N: 5;
numbers: [1,2,3,4,5];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
4;
```

# 💡 풀이

## ✍️ 풀이과정

DP 너무 어렵다. 고민하다가 결국 찾아봄. 작은걸로 하나하나 해야하는데 항상 접근을 못하겠음.

## 📖내가 작성한 JS Code

```javascript
function solution(N, number) {
    if (N === number) return 1;
    const dp = Array.from({length:9},()=> new Set());
    
    for (let i = 1; i<9;i++){
        dp[i].add(Number(String(N).repeat(i)));
        for (let j = 1; j< i; j++){
            for (const a of dp[j]){
                for (const b of dp[i-j]){
                    dp[i].add(a + b);
                    dp[i].add(a - b);
                    dp[i].add(b - a);
                    dp[i].add(a * b);
                    if (b !== 0) dp[i].add(Math.trunc(a / b));
                    if (a !== 0) dp[i].add(Math.trunc(b / a)); 
                    }
            }
        }
        if (dp[i].has(number)) return i;
    }
    return -1;
}
```

# 🧠 코드 리뷰

 - 장점
   - `dp[i]`를 "N을 i번 사용해 만들 수 있는 수의 집합(Set)"으로 정의한 전형적인 DP(Set-composition) 방식이 정확합니다.
   - `Number(String(N).repeat(i))`로 `N, NN, NNN ...`을 선행 추가해 탐색의 시작점을 잘 잡았습니다.
   - 사칙연산 생성 시 `a - b`, `b - a`를 모두 고려하고, 나눗셈에서 `Math.trunc`로 0을 향한 몫 처리 및 0 나눗셈 방지를 한 점이 문제 정의와 일치합니다.

 - 개선 제안/리팩터링 포인트
   1) 조기 종료의 범위를 루프 내부로 확대 (미세 최적화)
      - 현재는 각 단계가 끝난 뒤 `dp[i].has(number)`만 확인합니다. 새 값을 `dp[i]`에 추가하는 순간 목표값과 일치하는지 즉시 검사하고 발견되면 중첩 루프를 탈출하면 약간의 시간을 절약할 수 있습니다.

   2) 교환법칙(+/*)을 이용한 연산 수 절감 (선택)
      - `+`, `*`는 교환법칙이 성립하므로 `j`를 `1..i-1` 대신 `1..⌊i/2⌋`로 줄여 일부 연산을 감소시킬 수 있습니다. 다만 `-`, `/`은 비가환이므로 별도 처리가 필요해 코드가 복잡해질 수 있습니다. 현재처럼 단순하게 모두 생성하고 Set으로 중복을 제거하는 방식도 충분히 합리적입니다.

   3) 상수/변수 이름 정리로 가독성 향상
      - 최대 사용 횟수 8, DP 배열 길이 9 같은 매직 넘버를 상수로 분리하면 의도가 더 명확해집니다. 예: `const LIMIT = 8; const DP_LEN = LIMIT + 1;`
      - `a`, `b` 대신 `left`, `right` 등 의미가 드러나는 이름을 쓰고, `dp`는 `candidates`처럼 목적을 드러내면 읽기 쉬워집니다.

   4) 값 폭증에 대한 주의 (메모)
      - 조합되며 값의 개수가 급증할 수 있습니다. 값 범위 제한 등으로 Set 크기를 줄이는 시도는 정답을 누락할 위험이 있으므로 권장하지 않습니다. 현 구현은 안전하고 통과에 충분합니다.

   5) 정확성 체크포인트
      - `Math.trunc`는 음수에 대해서도 0을 향해 버림하므로 문제의 몫 정의에 부합합니다.
      - 0 나눗셈 방지를 `a !== 0`, `b !== 0`로 각각 둔 점이 적절합니다.
      - `if (N === number) return 1;` 기저 사례 처리가 명확합니다.

 - 요약
   - 접근은 올바르고, 실전에서도 충분히 통과 가능한 성능입니다. “중첩 루프 내 조기 종료”와 “상수/이름 정리” 정도만 적용해도 가독성과 미세 성능이 개선됩니다. 교환법칙을 활용한 연산 축소는 선택 사항이며, 단순성을 유지하고 싶다면 현 구조로도 충분합니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250906js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42895)

# 🖱️참고 링크

[MDN- has](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set/has)

<br>
[MDN- Set](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)

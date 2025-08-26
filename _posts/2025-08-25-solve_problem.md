---
layout: single
title: "[Programmers] 42842/카펫/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 완전탐색

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42842)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| brown | yellow |
| --- | --- |
| 10 | 2 |
| 8 | 1 |
| 24 | 24 |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| --- |
| [4, 3] |
| [3, 3] |
| [8, 6] |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
brown: 10
yellow: 2
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
[4,3]
```

# 💡 풀이

## ✍️ 풀이과정

더 좋은 방법이 없을까 하다가 그냥 완전 탐색이길래 이렇게 풀었음.

## 📖내가 작성한 JS Code

```javascript
function solution(brown, yellow) {
    const MAXNUMBER = brown+yellow;
    for (let i = 1; i<MAXNUMBER;i++){
        if (MAXNUMBER / i !== ~~(MAXNUMBER/i)) continue;
        if (MAXNUMBER/i * 2 + i*2 - 4 === brown) return [MAXNUMBER/i,i];
    }
}
```

# 🧠 코드 리뷰

- __가독성 개선__: `~~(MAXNUMBER / i)` 같은 비트 연산을 이용한 내림은 의도가 잘 드러나지 않습니다. `%` 연산자로 정수성(나누어떨어짐)을 확인하는 편이 명확합니다.
- __불필요한 탐색 감소__: `i`를 1부터 `T`까지 모두 순회하는 대신 약수 범위(최대 √T)만 확인하면 충분합니다.
- __문제 제약 반영__: 최소 테두리를 위해 `w, h`는 최소 3 이상이어야 합니다. 탐색 시작점을 3으로 설정합니다.
- __반환 형식__: 문제에서 가로가 세로보다 크거나 같아야 하므로 `w >= h` 를 보장하는 순회(예: `h`를 작은 쪽으로 두고 증가)로 일관성을 확보합니다.

### ✅ 개선된 JS 코드 (정수 약수 탐색, O(√T))

```javascript
function solution(brown, yellow) {
  const T = brown + yellow;
  for (let h = 3; h <= Math.floor(Math.sqrt(T)); h++) {
    if (T % h !== 0) continue; // h가 T의 약수가 아니면 패스
    const w = T / h; // 정수 보장
    if ((w - 2) * (h - 2) === yellow) return [w, h];
  }
}
```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250825js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42842)

# 🖱️참고 링크

[MDN- for(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for)

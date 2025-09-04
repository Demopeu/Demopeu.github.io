---
layout: single
title: "[Programmers] 42884/단속카메라/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42884)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| routes                                    |
| ----------------------------------------- |
| [[-20,-15], [-14,-5], [-18,-13], [-5,-3]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 2      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 차량의 대수는 1대 이상 10,000대 이하입니다.
- routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
- 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
- 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
routes: [
  [-20, -15],
  [-14, -5],
  [-18, -13],
  [-5, -3],
];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
2;
```

# 💡 풀이

## ✍️ 풀이과정

차량 진출 지점을 기준으로, 다음 차량 진입 지점이 크면 설치하면 됨. 간단한 문제.

## 📖내가 작성한 JS Code

```javascript
function solution(routes) {
  return routes
    .sort((a, b) => a[1] - b[1])
    .reduce(
      (acc, cur) => {
        if (acc[0] < cur[0]) {
          acc[1]++;
          acc[0] = cur[1];
        }
        return acc;
      },
      [-Infinity, 0]
    )[1];
}
```

# 🧠 코드 리뷰

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250904js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42884)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

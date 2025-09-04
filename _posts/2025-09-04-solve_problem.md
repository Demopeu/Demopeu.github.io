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

* __그리디 전략 타당성__: `종료 지점(나간 지점)` 기준 오름차순 정렬 후, 현재 카메라 위치보다 `다음 차량의 진입 지점`이 크면 해당 차량의 `진출 지점`에 카메라를 설치하는 전형적인 간격 스케줄링(최대 비겹침 구간) 그리디입니다. 최적성 조건을 만족합니다.
* __시간/공간 복잡도__: 정렬 O(N log N) + 1패스 O(N)로 전체 O(N log N). 추가 공간 O(1). 최적입니다.
* __정확성__: 문제의 규칙상 "진입/진출 지점에 카메라가 있어도 만난 것으로 간주"되므로 비교를 `acc[0] < cur[0]`(엄격 부등)으로 둔 점이 정확합니다. `acc[0] === cur[0]`인 경우 새 카메라 불필요합니다.
* __초기값 처리__: `[-Infinity, 0]`으로 시작해 첫 구간에서 설치를 보장합니다. `routes.length === 0`일 때 `0`을 반환하기 때문에 경계값도 안전합니다.
* __가독성 제안(선택)__:
  - 누적자 배열 의미를 드러내는 네이밍 또는 구조 분해를 권장합니다. 예: `[camera, count]` 또는 `{ pos, count }`.
  - `Number.NEGATIVE_INFINITY` 명시 사용으로 의도 표현을 강화할 수 있습니다.

### 리팩터링 예시(동일 로직, 가독성 강화)

```javascript
function solution(routes) {
  routes.sort((a, b) => a[1] - b[1]);
  let camera = Number.NEGATIVE_INFINITY;
  let count = 0;
  for (const [start, end] of routes) {
    if (camera < start) {
      camera = end;
      count++;
    }
  }
  return count;
}
```

* __테스트 팁__: 동일 지점 진입·진출이 많은 케이스, 완전 중첩/부분 중첩 구간, 음수/양수 혼재 구간, 빈 배열을 포함해 검증하면 좋습니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250904js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42884)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

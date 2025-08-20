---
layout: single
title: "[Programmers] 42583/트럭/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 자료구조
  - 구현
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

프로그래머스라 문제 설명은 링크로 대체

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| bridge_length | weight | truck_weights                   |
| ------------- | ------ | ------------------------------- |
| 2             | 10     | [7,4,5,6]                       |
| 100           | 100    | [10]                            |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 8      |
| 101    |
| 100    |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- bridge_length는 1 이상 2000 이하입니다.
- weight는 1 이상 10000 이하입니다.
- truck_weights의 길이는 1 이상 10000 이하입니다.
- truck_weights의 모든 수는 1 이상 weight 이하입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
bridge_length: 2
weight: 10
truck_weights: [7,4,5,6]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
8
```

# 💡 풀이

## ✍️ 풀이과정

이거 2년전에도 못풀었는데 지금도 못풀겠음. 내 공부 방법이 잘못된게 아닌가 생각중임. 이번엔 reduce로 풀었는데, 현재 시간을 저장하면 풀었는데 그 생각을 못했음. 생각하는 힘을 좀 더 길러야하지 않나...

## 📖내가 작성한 JS Code

```javascript
function solution(bridge_length, weight, truck_weights) {
  return truck_weights.reduce(
    (acc, cur) => {
      let { time, sum, q, lastExit } = acc;

      while (true) {
        if (q.length && q[0].exit === time) {
          sum -= q.shift().cur;
          continue;
        }
        if (sum + cur <= weight) {
          const exit = time + bridge_length;
          q.push({ cur, exit });
          sum += cur;
          time += 1;
          if (exit > lastExit) lastExit = exit;
          return { time, sum, q, lastExit };
        }
        time = q[0].exit;
      }
    },
    { time: 1, sum: 0, q: [], lastExit: 0 }
  )["lastExit"];
}
```

# 🧠 코드 리뷰

# 요약

- **정확성**: `exit = time + bridge_length`와 `lastExit` 추적으로 총 소요시간을 정확히 계산합니다.
- **효율성**: 진입 불가 시 `time = q[0].exit`로 점프해 불필요한 1초 단위 시뮬레이션을 줄였습니다.
- **주의점**: 배열 `shift()`는 O(n)입니다. 큐 길이가 길어지면 비용이 커지니 head 포인터로 O(1) pop을 권장합니다.
- **가독성**: `reduce` 기반 상태관리는 난이도가 높습니다. 일반 `while` 루프가 더 직관적입니다.
- **명명/주석**: `q`→`queue`, `cur`→`weight` 등 의미 있는 이름과 블록 주석을 추가하세요.
- **문서화**: 예제 3건의 입력/출력 결과 표를 본문에 함께 제시하면 신뢰도가 높아집니다.

**권장 한 줄 요약**: shift()를 없애는 head 포인터 기반 큐와 while 루프로 리팩터링해 가독성과 성능을 동시에 개선하세요.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250820js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

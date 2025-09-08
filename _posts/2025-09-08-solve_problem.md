---
layout: single
title: "[Programmers] 43162/네트워크/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 너비우선탐색

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/43162)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n   | computers                         |
| --- | --------------------------------- |
| 3   | [[1, 1, 0], [1, 1, 0], [0, 0, 1]] |
| 2   | [[1, 1, 0], [1, 1, 1], [0, 1, 1]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 2      |
| 1      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
n: 3;
computers: [
  [1, 1, 0],
  [1, 1, 0],
  [0, 0, 1],
];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
2;
```

# 💡 풀이

## ✍️ 풀이과정

그냥 BFS 갯수 세면 안됨? 해서 그냥 해봄. 그런데 통과하고 보니 왜 routes를 쓰는지 모르겠음. 좀 더 생각하고 풀었어야했는데, 부끄러운 마음에 박제해놓음.

## 📖내가 작성한 JS Code

```javascript
function solution(n, computers) {
  let answer = 0;
  const visited = Array(n + 1).fill(false);
  const q = [];
  const routes = computers.reduce(
    (acc, cur, idx) => {
      for (let i = 0; i < n; i++) {
        if (i === idx) continue;
        if (cur[i]) {
          acc[i].add(idx);
          acc[idx].add(i);
        }
      }
      return acc;
    },
    Array.from({ length: n }, () => new Set())
  );

  for (let i = 0; i < n; i++) {
    if (visited[i]) continue;
    q.push([i]);
    visited[i] = true;
    while (q.length) {
      const cur = q.shift();
      for (const com of [...routes[cur]]) {
        if (!visited[com]) {
          visited[com] = true;
          q.push(com);
        }
      }
    }
    answer++;
  }
  return answer;
}
```

# 🧠 코드 리뷰

- 장점

  - 인접 행렬(`computers`)을 인접 리스트(`routes: Set[]`)로 변환해 중복 간선을 제거하고, BFS로 연결 요소(네트워크) 개수를 세는 정석적 접근입니다.
  - 자기 자신(`i === idx`)을 제외한 간선만 추가하여 셀프 루프를 방지한 점이 명확합니다.
  - `Set`을 사용하여 동일 간선 중복 추가를 피한 것은 좋습니다.

- 버그(필수 수정)

  1. 큐 원소 타입 불일치로 인한 런타임 오류 가능
     - 초기 삽입 시 `q.push([i])`로 배열을 넣고, 이후에는 `q.push(com)`으로 숫자를 넣습니다. `shift()` 후 `const cur = q.shift();`에서 `cur`가 배열/숫자가 섞여 `routes[cur]` 인덱싱이 깨집니다.
     - 수정: 큐에는 항상 숫자만 넣으세요. 예) 시작은 `q.push(i)`, 내부도 `q.push(next)`처럼 일관 유지.
  2. `for (const com of [...routes[cur]])`의 스프레드는 불필요하며, 변수명도 의미가 모호합니다. `for (const next of routes[cur])`로 바로 순회하세요.

- 개선 제안/리팩터링 포인트

  1. 방문 배열 크기: `Array(n + 1)`는 과합니다. 인덱스가 0..(n-1)이므로 `Array(n)`으로 충분합니다.
  2. 큐 성능: `Array.prototype.shift()`는 O(N)입니다. 헤드 포인터를 두는 방식으로 바꾸면 BFS가 O(V+E)에 가깝게 동작합니다.
  3. 메모리 절약 대안: 인접 리스트를 만들지 않고 인접 행렬을 직접 순회하며 BFS를 수행하면 `Set`/배열 생성 비용을 줄일 수 있습니다(시간·공간 트레이드오프).
  4. 네이밍: `routes`, `q`, `com` 등은 목적이 드러나도록 `adj`, `queue`, `next` 등으로 개선하면 가독성이 좋아집니다.

- 정확성

  - 각 미방문 정점에서 시작한 BFS가 끝날 때마다 `answer++`를 수행하면 연결 요소의 개수를 정확히 세게 됩니다(무가중치 그래프에서 연결성 판단은 BFS로 충분).

- 복잡도

  - 인접 리스트 구축: O(n^2) — 인접 행렬 전체를 한 번 스캔
  - BFS: O(V + E) — 포인터 큐 사용 시
  - 공간: 인접 리스트 O(V + E), 방문 배열 O(V)

- 엣지 케이스 체크리스트

  - 완전 분리 그래프: 대각만 1이고 나머지 0인 경우 → 결과는 n
  - 완전 연결 그래프: 모든 i != j가 1인 경우 → 결과는 1
  - 비대칭 입력(이상치): `computers[i][j] !== computers[j][i]`인 행렬이 들어오면 현재 양방향으로 보정하여 `routes`를 만들기 때문에 연결성 판단엔 문제 없음

- 리팩터링 예시(동일 정답, 버그 수정 + 큐 포인터 + 가독성)

```javascript
function solution(n, computers) {
  let components = 0;
  const visited = Array(n).fill(false);

  // 인접 리스트 구성 (양방향)
  const adj = Array.from({ length: n }, () => new Set());
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      if (i !== j && computers[i][j]) {
        adj[i].add(j);
        adj[j].add(i);
      }
    }
  }

  // BFS (포인터 큐)
  const queue = new Array(n * n);
  for (let start = 0; start < n; start++) {
    if (visited[start]) continue;
    let head = 0,
      tail = 0;
    queue[tail++] = start;
    visited[start] = true;
    while (head < tail) {
      const cur = queue[head++];
      for (const next of adj[cur]) {
        if (!visited[next]) {
          visited[next] = true;
          queue[tail++] = next;
        }
      }
    }
    components++;
  }
  return components;
}
```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250908js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/43162)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

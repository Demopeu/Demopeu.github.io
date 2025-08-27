---
layout: single
title: "[Programmers] 86971/전력망을 둘로 나누기/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 완전탐색
  - dfs

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/86971)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n   | wires                                             |
| --- | ------------------------------------------------- |
| 9   | [[1,3],[2,3],[3,4],[4,5],[4,6],[4,7],[7,8],[7,9]] |
| 4   | [[1,2],[2,3],[3,4]]                               |
| 7   | [[1,2],[2,7],[3,7],[3,4],[4,5],[6,7]]             |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 3      |
| 0      |
| 1      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- n은 2 이상 100 이하인 자연수입니다.
- wires는 길이가 n-1인 정수형 2차원 배열입니다.
  - wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.
  - 1 ≤ v1 < v2 ≤ n 입니다.
  - 전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
n: 9
wires: [[1,3],[2,3],[3,4],[4,5],[4,6],[4,7],[7,8],[7,9]]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
3
```

# 💡 풀이

## ✍️ 풀이과정

생각나는데로 적어봤는데, 하고 보니까 이거 너무 많은 메모리를 씀.

아래 코드가 더 좋은듯. 일단 부모와 그 아래 자식을 한번에 저장한 다음 하면 여러번 안해도 됨.

```javascript
function solution(n, wires) {
  const g = Array.from({ length: n + 1 }, () => []);
  for (const [a, b] of wires) {
    g[a].push(b);
    g[b].push(a);
  }

  const parent = Array(n + 1).fill(0);
  const sub = Array(n + 1).fill(1);
  const visited = Array(n + 1).fill(false);

  function dfs(u) {
    visited[u] = true;
    for (const v of g[u]) {
      if (!visited[v]) {
        parent[v] = u;
        dfs(v);
        sub[u] += sub[v];
      }
    }
  }
  dfs(1);

  let answer = Infinity;
  for (const [a, b] of wires) {
    const child = parent[a] === b ? a : b;
    const diff = Math.abs(n - 2 * sub[child]);
    if (diff < answer) answer = diff;
  }

  return answer;
}
```

## 📖내가 작성한 JS Code

```javascript
function solution(n, wires) {
  let answer = Infinity;
  for (let i = 0; i < n; i++) {
    const newWires = [...wires.slice(0, i), ...wires.slice(i + 1)];
    const visited = Array(n + 1).fill(false);
    visited[1] = true;
    const graph = Array.from({ length: n + 1 }, () => []);
    for (wire of newWires) {
      graph[wire[0]].push(wire[1]);
      graph[wire[1]].push(wire[0]);
    }
    const q = [1];
    while (q.length) {
      const node = q.shift();
      for (nod of graph[node]) {
        if (!visited[nod]) {
          q.push(nod);
          visited[nod] = true;
        }
      }
    }
    const count = visited.reduce((acc, cur) => acc + cur, 0);
    answer = Math.min(answer, Math.abs(n - count - count));
  }
  return answer;
}
```

# 🧠 코드 리뷰

# 요약

- **루프 범위 버그**: 간선을 제거하며 순회해야 하므로 `for (let i = 0; i < wires.length; i++)`가 맞습니다.
- **암묵적 전역 변수**: `for (wire of ...)`, `for (nod of ...)`는 `const/let` 누락. `for (const wire of ...)`, `for (const nod of ...)`로 수정.
- **큐 성능**: `q.shift()`는 O(n). 인덱스 포인터(`let head = 0; const node = q[head++];`)로 O(1) 처리 권장.
- **불필요한 복사**: 매 반복 `newWires` 생성 대신 그래프는 한 번만 만들고, 탐색 시 해당 간선만 스킵.
- **시작 노드 명시성**: 항상 1에서 시작하기보다 끊은 간선의 한쪽(`wires[i][0]`)에서 시작하면 더 명확.
- **가독성**: `reduce` boolean 합산은 동작하나 모호. `a + (c ? 1 : 0)`로 명시화 권장.

# 권장 최소 수정

- `for (let i = 0; i < wires.length; i++)`
- `for (const wire of newWires)` / `for (const nod of graph[node])`
- 큐를 인덱스 기반으로 변경: `let head = 0; while (head < q.length) { const node = q[head++]; ... }`
- 가능하면 그래프 1회 구성 + 간선 스킵 방식으로 전환
- `const count = visited.reduce((a, c) => a + (c ? 1 : 0), 0)`

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250827js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/86971)

# 🖱️참고 링크

[MDN- 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

---
layout: single
title: "[Programmers] 49189/가장 먼 노드/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 그래프
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/43105)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n   | vertex                                           |
| --- | ------------------------------------------------ |
| 6   | [[3, 6], [4, 3], [3, 4], [2, 3], [5, 4], [4, 5]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 3      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 노드의 개수 n은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
vertex: [
  [3, 6],
  [4, 3],
  [3, 4],
  [2, 3],
  [5, 4],
  [4, 5],
];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
3;
```

# 💡 풀이

## ✍️ 풀이과정

왜 레벨 3인지 모르겠는 문제. 그냥 그래프 그릴 줄 알면 끝!

## 📖내가 작성한 JS Code

```javascript
function solution(n, edge) {
  const graph = edge.reduce(
    (acc, cur) => {
      acc[cur[0]].push(cur[1]);
      acc[cur[1]].push(cur[0]);
      return acc;
    },
    Array.from({ length: n + 1 }, () => [])
  );

  const q = [[1, 0]];
  const visited = Array(n + 1).fill(false);
  let MAXCOUNT = 0;
  let answer = [];
  visited[1] = true;

  while (q.length) {
    const [node, count] = q.shift();
    if (count > MAXCOUNT) {
      MAXCOUNT = count;
      answer = [node];
    } else if (count === MAXCOUNT) answer.push(node);
    for (const n of graph[node]) {
      if (!visited[n]) {
        q.push([n, count + 1]);
        visited[n] = true;
      }
    }
  }
  return answer.length;
}
```

# 🧠 코드 리뷰

 - 장점
   - `edge`를 인접 리스트로 변환해 BFS로 최장 거리(레벨)를 탐색하는 정석적 접근입니다.
   - 방문 여부를 큐에 넣는 시점에 바로 `visited` 처리하여 중복 방문을 방지한 점이 좋습니다.
   - 최대 레벨 갱신 시 동레벨 노드를 배열에 담고 최종 길이를 반환하는 방식은 문제 요구사항에 정확하게 부합합니다.

 - 개선이 필요한 부분/권장 리팩터링
   1) 큐 구현 성능: `Array.prototype.shift()`는 O(N)이라 전체가 O(V^2)에 가까워질 수 있습니다. 포인터(헤드 인덱스)를 두고 `q[head++]`로 꺼내면 BFS가 진짜 O(V+E)로 동작합니다.
   2) 변수명 가독성: 파라미터 `n`과 `for (const n of graph[node])`의 반복 변수명이 겹쳐 가독성이 떨어집니다. 반복 변수는 `next` 또는 `neighbor` 등으로 변경을 권장합니다.
   3) 상수 네이밍: `MAXCOUNT`는 변경되는 값이므로 상수 느낌의 대문자 네이밍보다 `maxDepth` 같은 카멜표기를 사용하는 편이 의도를 잘 드러냅니다.
   4) 레벨 계산 방식: 현재 로직도 정답이지만, 레벨 단위로 큐를 처리하여 “마지막 레벨의 크기”를 바로 반환하면 배열 조작을 줄일 수 있습니다.

 - 정확성
   - 무가중치 그래프에서 BFS는 시작점(1)으로부터의 최단 레벨을 보장합니다. 최대 레벨에 속한 노드 개수를 세면 정답입니다.

 - 복잡도
   - 그래프 구축: O(E)
   - BFS: 큐 포인터 방식 사용 시 O(V+E), 현재 `shift` 사용 시 최악에선 추가 오버헤드 발생 가능
   - 공간: 인접 리스트 O(V+E), 방문 배열 O(V)

 - 엣지 케이스 제안
   - `n = 2`, 간선 1개(가장 단순한 그래프)
   - 스타 그래프(1에서 모든 노드로 바로 연결) → 결과는 맨 마지막 레벨(1-hop)의 노드 수 = V-1
   - 사슬형 그래프(선형) → 결과는 1개
   - 비연결 그래프 입력 시(문제 특성상 보통 연결이지만), 시작점으로부터 도달 불가 노드는 무시됩니다.

 - 리팩터링 예시(동일 정답, 큐 포인터/가독성 개선)

   ```javascript
   function solution(n, edge) {
     const graph = Array.from({ length: n + 1 }, () => []);
     for (const [a, b] of edge) {
       graph[a].push(b);
       graph[b].push(a);
     }

     const visited = Array(n + 1).fill(false);
     const q = new Array(n + 5);
     let head = 0, tail = 0;
     const push = (v, d) => (q[tail++] = [v, d]);
     const pop = () => q[head++];

     push(1, 0);
     visited[1] = true;

     let maxDepth = 0;
     let countAtMax = 0;

     while (head < tail) {
       const [node, depth] = pop();
       if (depth > maxDepth) {
         maxDepth = depth;
         countAtMax = 1;
       } else if (depth === maxDepth) {
         countAtMax++;
       }
       for (const neighbor of graph[node]) {
         if (!visited[neighbor]) {
           visited[neighbor] = true;
           push(neighbor, depth + 1);
         }
       }
     }
     return countAtMax;
   }
   ```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250906js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/49189)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

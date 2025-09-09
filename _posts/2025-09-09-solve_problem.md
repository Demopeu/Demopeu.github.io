---
layout: single
title: "[Programmers] 49191/순서/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 플로이드워셜

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/49191)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n   | result                         |
| --- | --------------------------------- |
| 5   | [[4, 3], [4, 2], [3, 2], [1, 2], [2, 5]]|

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 2      |
| 1      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 선수의 수는 1명 이상 100명 이하입니다.
- 경기 결과는 1개 이상 4,500개 이하입니다.
- results 배열 각 행 [A, B]는 A 선수가 B 선수를 이겼다는 의미입니다.
- 모든 경기 결과에는 모순이 없습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
n: 5;
results: [
  [4, 3],
  [4, 2],
  [3, 2],
  [1, 2],
  [2, 5],
];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
2;
```

# 💡 풀이

## ✍️ 풀이과정

어떻게 푸는지 모르겠다. 그래서 조금 찾아봄. 플로이드 워셀로 그냥 전부 다 연결하는게 답이라고 해서 뇌가 굳었나 싶음. 그리고 둘 다 모르거나 둘 다 아는 경우 빼고 다 더해서 알면 순위를 아느

## 📖내가 작성한 JS Code

```javascript
function solution(n, results) {
    const graph = results.reduce((acc,[winner,loser])=>{
        acc[winner][loser]= true;
        return acc;
    },Array.from({ length: n+1 }, () => Array(n+1).fill(false)));
    
    for (let k = 1;k<n+1;k++){
        for (let i = 1;i<n+1;i++){
            for (let j = 1;j<n+1;j++){
                if (graph[i][k] && graph[k][j]) {
                    graph[i][j] = true;
                }
            }
        }
    }
    let answer = 0;
    for (let i = 1; i <= n; i++) {
        let known = 0;
        for (let j = 1; j <= n; j++) {
            if (i === j) continue;
            if (graph[i][j] ^ graph[j][i]) known++;
        }
        if (known === n - 1) answer++;
    }
  return answer;
}
```

# 🧠 코드 리뷰
- 알고리즘: 인접 행렬에서 전이 폐쇄를 구하는 플로이드–워셜(Floyd–Warshall) 방식으로 승패를 유추합니다. `if (graph[i][k] && graph[k][j]) graph[i][j] = true;`가 핵심이며 문제 요구사항을 충족합니다.
- 정확성: 각 선수 `i`에 대해 다른 모든 선수 `j`와의 우열이 하나만 참(둘 중 하나만 true)일 때 `known === n - 1`로 순위를 확정하는 로직이 타당합니다.
- 복잡도: 시간 O(n^3), 공간 O(n^2)으로 `n ≤ 100` 제약에서 충분히 효율적입니다. 대안으로 인접 리스트 + BFS/DFS를 모든 정점에 대해 수행(O(n*(n+E)))하는 방법도 있습니다.
- 초기화: `Array.from({ length: n+1 }, () => Array(n+1).fill(false))`로 1-based 인덱스를 일관되게 사용하고, `results`를 반영해 `winner -> loser`를 `true`로 설정한 부분이 명확합니다.
- 스타일: `graph[i][j] ^ graph[j][i]`는 동작은 하지만 가독성이 낮습니다. 유지보수성을 위해 `graph[i][j] !== graph[j][i]`로 의도를 드러내면 더 읽기 좋습니다.
- 미세 최적화: 삼중 루프에서 `if (!graph[i][k]) continue;`로 가지치기를 넣으면 평균적으로 약간의 이득이 있습니다. 결과에는 영향 없습니다.
- 설명 보완: 본문 풀이 설명에 “중간 노드 k를 거쳐 전이 관계를 채운 뒤, 양방향이 모두 true/false인 경우(우열 미정/동치)만 제외하고 나머지가 모두 판별되면 순위 확정”이라는 요지를 한 문장으로 명시하면 독자가 흐름을 더 빨리 이해할 수 있습니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250909js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/49191)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

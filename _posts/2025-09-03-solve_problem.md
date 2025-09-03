---
layout: single
title: "[Programmers] 42861/섬 연결하기/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 그리디알고리즘
  - 크루스칼알고리즘

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42861)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n | costs  |
| --- | ----- |
| 4  | [[0,1,1],[0,2,2],[1,2,3],[1,3,1],[2,3,1]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 4      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 ((n-1) * n) / 2이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
n: 4
k: [[0,1,1],[0,2,2],[1,2,3],[1,3,1],[2,3,1]]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
4
```

# 💡 풀이

## ✍️ 풀이과정

간만에 하니까 크루스칼 알고리즘이 뭔지 모르겠드라. union-find를 사용하는 건데 솔직히 기억이 안나서 먼저 다시 개념 공부하고 다시 풀었음. 아 이거 무슨 알고리즘이였는데 했었기 때문. 원리만 알면 쉬운 문제.

## 📖내가 작성한 JS Code

```javascript
function solution(n, costs) {
    const parent = Array.from({length:n},(_,idx)=>idx);
    
    const findParent = (node) =>{
        if (parent[node] === node){
            return node;
        }
        return parent[node] = findParent(parent[node]) 
    }
    
    const unionParent = (a,b)=>{
        const rootA = findParent(a);
        const rootB = findParent(b);
        rootA < rootB ? parent[rootB] = rootA: parent[rootA] = rootB;
    }
    
    let totalCost = 0;
    let edgeCount = 0;
    
    for (const e of costs.sort((a,b)=>a[2]-b[2])) {
    const [x, y, z] = e;
    
    if (findParent(x) !== findParent(y)) {
        unionParent(x, y);
        totalCost += z;
        edgeCount++;
    }
    
    if (edgeCount === n - 1) break;
}
    
    return totalCost;
}
```

# 🧠 코드 리뷰
# 개요
- __알고리즘__: 크루스칼(MST) 정석 구현. 간선 비용 오름차순 정렬 후, 서로소 집합으로 사이클 방지하며 `n-1`개 간선 선택.
- __복잡도__: 정렬 O(E log E) + 거의 O(E α(N)) 유니온파인드. 제약(n≤100)에서 충분히 빠름.

# 정확성 체크
- __사이클 방지__: `findParent(x) !== findParent(y)` 조건으로 적절히 차단됨.
- __조기 종료__: `edgeCount === n-1`에서 break로 불필요 연산 배제, OK.
- __경계값__: 연결 불가 경우가 입력으로 주어지지 않는다는 제약을 가정. 만약 일반화한다면, 최종 `edgeCount < n-1`일 때 예외/검증 로직 고려.

# 코드 품질/개선 포인트
- __입력 변이 방지__: `costs.sort(...)`는 원본 배열을 변이합니다. 함수 외부에서 `costs` 재사용 시 부작용 우려가 있으니, 아래처럼 복사본 정렬 권장.
  ```javascript
  for (const [x, y, z] of [...costs].sort((a, b) => a[2] - b[2])) {
    // ...
  }
  ```
- __union by rank/size__: 현재는 루트 번호 크기로 합침. 트리 높이 최소화를 위해 rank/size 기준으로 union하면 find가 더 안정적으로 O(α(N))에 근접.
- __가독성 정리__: 구조 분해를 `for...of` 헤더에서 바로 수행하면 본문이 간결해집니다. 세미콜론/공백 일관성도 유지 권장.
- __네이밍__: `findParent`, `unionParent`는 명확함. `parent`는 `parentOrRoot` 등으로 의도 명시도 가능하나 현재도 충분히 직관적.

# 제안하는 정리 버전(부작용 제거 + rank 적용)
```javascript
function solution(n, costs) {
  const parent = Array.from({ length: n }, (_, i) => i);
  const rank = Array(n).fill(0);

  const find = (x) => (parent[x] === x ? x : (parent[x] = find(parent[x])));

  const unite = (a, b) => {
    let ra = find(a), rb = find(b);
    if (ra === rb) return false;
    if (rank[ra] < rank[rb]) [ra, rb] = [rb, ra];
    parent[rb] = ra;
    if (rank[ra] === rank[rb]) rank[ra]++;
    return true;
  };

  let total = 0, used = 0;
  for (const [x, y, w] of [...costs].sort((a, b) => a[2] - b[2])) {
    if (unite(x, y)) {
      total += w;
      if (++used === n - 1) break;
    }
  }
  return total;
}
```

# 추가 메모
- 문서의 예제 입력 블록에서 키 이름이 `k`로 표기되어 있는데, 실제 문제의 두 번째 파라미터는 `costs`입니다. 문서 표기만 오탈자 수정 권장(코드에는 영향 없음).

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250903js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42861)

# 🖱️참고 링크

[나무위키-크루스칼알고리즘](https://namu.wiki/w/%ED%81%AC%EB%A3%A8%EC%8A%A4%EC%B9%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

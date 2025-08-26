---
layout: single
title: "[Programmers] 87946/피로도/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/87946)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| k | dungeons |
| --- | --- | 
| 80 | [[80,20],[50,40],[30,10]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| --- |
| 3 |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- k는 1 이상 5,000 이하인 자연수입니다.
- dungeons의 세로(행) 길이(즉, 던전의 개수)는 1 이상 8 이하입니다.
  - dungeons의 가로(열) 길이는 2 입니다.
  - dungeons의 각 행은 각 던전의 ["최소 필요 피로도", "소모 피로도"] 입니다.
  - "최소 필요 피로도"는 항상 "소모 피로도"보다 크거나 같습니다.
  - "최소 필요 피로도"와 "소모 피로도"는 1 이상 1,000 이하인 자연수입니다.
  - 서로 다른 던전의 ["최소 필요 피로도", "소모 피로도"]가 서로 같을 수 있습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
k: 80
dungeons: [[80,20],[50,40],[30,10]]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
3
```

# 💡 풀이

## ✍️ 풀이과정

dfs로 풀었음. 파이썬이랑 다른 점은 gobal 안써도 되는거임. 둘 다 정적 스코프 언어인데, 파이썬은 엄격하기 때문에 그 지역에 answer이 없으면 오류를 내뿜는다. 하지만, js는 클로저로 인해 상위 환경을 다 찾아보기 때문에 global필요 없음.

## 📖내가 작성한 JS Code

```javascript
function solution(k, dungeons) {
    const LEN = dungeons.length
    let answer = 0;
    const visited = Array(LEN).fill(false);
    
    const dfs = (cur,count)=>{
        answer = Math.max(answer,count);
        
        for (let i = 0; i<LEN;i++){
            const [need, used] =  dungeons[i];
            if(!visited[i] && cur>=need){
                visited[i] = true;
                dfs(cur-used, count+1);
                visited[i] = false;
            }
        }
    }
    
    dfs(k,0);
    return answer;
}
```

# 🧠 코드 리뷰

- __정확성__: 백트래킹(DFS)로 가능한 모든 순서를 탐색하며 `answer`를 매 단계에서 `Math.max`로 갱신. 방문 처리와 해제(`visited[i] = true/false`)가 올바르게 대칭이라 누락/중복 없이 최대 탐험 수를 계산합니다.
- __경계 상황__: 던전 수가 0이거나(`LEN === 0`), 모든 던전의 최소 필요 피로도보다 `k`가 작을 때도 `answer = 0`으로 정상 반환됩니다. 동일한 [need, used]가 여러 개여도 인덱스 기반 방문 체크로 잘 동작합니다.
- __시간 복잡도__: 최악 O(n!)에 가깝지만 n ≤ 8이라 제한 내에서 충분합니다. 공간은 `visited` O(n) + 재귀 깊이 O(n).
- __JS 스코프 메모__: 전역이 필요 없다는 설명은 방향성은 맞습니다. 지금 구조는 바깥 스코프의 `answer`를 클로저로 캡처해서 재할당합니다. 참고로 파이썬에서는 내부 함수가 바깥 변수에 “재할당”할 때 `nonlocal`/`global`이 필요합니다(단순 참조만 하면 불필요). 현재 JS 구현은 자연스럽습니다.
- __가독성/스타일 개선 제안__:
  - 매개변수명: `cur`, `count` → 의미가 드러나게 `energy`, `cleared` 등 권장.
  - JSDoc 주석으로 입출력 명세를 짧게 남기면 좋습니다.
  - `LEN`은 상수로 적절. `for` 루프 선택도 중단/continue가 필요한 DFS 특성상 합당합니다.
- __성능/로직 개선 아이디어__(선택):
  - 가지치기: 남은 던전 수로도 현재 `answer`를 넘을 수 없으면 조기 종료.
  - 비트마스크 방문표시: `visited` 배열 대신 정수 마스크 사용으로 약간의 상수 개선과 메모리 절감(n ≤ 8이라 구현 간단).
- __예시 가지치기__:
```javascript
const dfs = (energy, cleared, usedMask) => {
  answer = Math.max(answer, cleared);
  const remaining = LEN - cleared; // 상한
  if (cleared + remaining <= answer) return; // 더 늘 수 없으면 중단
  for (let i = 0; i < LEN; i++) {
    if (usedMask & (1 << i)) continue;
    const [need, cost] = dungeons[i];
    if (energy >= need) dfs(energy - cost, cleared + 1, usedMask | (1 << i));
  }
};
```
- __테스트 권장 케이스__:
  - k가 매우 작아 어떤 던전도 못 가는 경우: k=1, dungeons=[[2,1]].
  - 모든 던전을 순회 가능한 경우: k가 충분히 큰 입력.
  - 동일한 [need, used]가 여러 개 있는 경우.
  - need == used, used가 1인 얕은 소모 케이스.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250826js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/87946)

# 🖱️참고 링크

[MDN- 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

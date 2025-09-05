---
layout: single
title: "[Programmers] 42898/등굣길/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42898)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| m | n | puddles     |
| -------- |
| 4 | 3 | [[2,2]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 4      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 격자의 크기 m, n은 1 이상 100 이하인 자연수입니다.
  - m과 n이 모두 1인 경우는 입력으로 주어지지 않습니다.
- 물에 잠긴 지역은 0개 이상 10개 이하입니다.
- 집과 학교가 물에 잠긴 경우는 입력으로 주어지지 않습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
m: 4;
n: 3;
puddles: [[2,2]];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
4;
```

# 💡 풀이

## ✍️ 풀이과정

set으로 그냥 찾으면 되지 않을까 했는데, js에서 has는 참조값으로 비교하기 때문에 이상하게 set만들어서 비교함. 다르게 했으면 더 깔끔했을듯?

## 📖내가 작성한 JS Code

```javascript
function solution(m, n, puddles) {
    const setPuddles = new Set(puddles.map(([x,y])=>`${y},${x}`));
    const answer = Array.from({length:n},()=>Array.from({length:m},()=>0));
    answer[0][0] = 1;
    answer.forEach((el,idx)=>{
        for(let i = 0; i<m;i++){
            if (setPuddles.has(`${idx+1},${i+1}`)) continue;
            el[i] += ((idx-1>=0? answer[idx-1][i]: 0) + (i-1>=0? el[i-1]:0))% 1000000007;
        }
    })
    return answer[n-1][m-1];
}
```

# 🧠 코드 리뷰

 - 장점
   - 격자형 경로 수를 구하는 전형적인 동적 계획법(DP) 풀이입니다. 위(Top)와 왼쪽(Left)에서 오는 경로 수의 합을 현재 칸에 누적하는 점화식이 정확합니다.
   - 장애물(`puddles`) 체크를 O(1)로 하기 위해 `Set`을 사용하는 접근은 합리적입니다. [x, y]를 `${y},${x}` 문자열 키로 변환하여 좌표를 일관되게 처리한 점도 좋습니다.

 - 개선 제안/리팩터링 포인트
   1) 모듈러 연산 위치와 표현
      - 현재 `el[i] += ((top) + (left)) % MOD` 형태인데, 의미상 대입을 분명히 하기 위해 `+=`보다는 `=`가 가독성이 좋습니다. 또한 합 전체에 모듈러를 적용하는 습관을 권장합니다.
      - 예: `el[i] = (top + left) % MOD;` (단, 시작점은 사전에 1로 설정되어 있으므로 현재 구조에서도 동작에는 문제가 없습니다.)

   2) 경계/시작점 처리 명확화
      - 시작점(0,0)은 이미 1로 세팅되어 있는데, 루프에서 다시 계산될 수 있으므로 다음과 같이 명시적으로 건너뛰면 의도 전달이 더 명확해집니다.
      - 예: `if (idx === 0 && i === 0) continue;`

   3) 가독성: 상수/변수 이름
      - `1000000007`은 매직 넘버이므로 상수로 분리하면 좋습니다. `const MOD = 1_000_000_007;`
      - `el`, `idx`, `i` 대신 `row`, `r`, `c` 등 의미가 드러나는 이름을 쓰면 가독성이 향상됩니다.

   4) 복잡도/공간 최적화 대안 (선택)
      - 현재는 2차원 배열 O(n·m) 공간을 사용합니다. 1차원 DP O(m)로 최적화가 가능합니다. 장애물만 체크하면 되므로 같은 행에서 왼쪽 값을 누적하고, 위쪽 값은 동일 인덱스의 이전 값이 됩니다.

      ```javascript
      function solution(m, n, puddles) {
        const MOD = 1_000_000_007;
        const blocked = new Set(puddles.map(([x, y]) => `${y},${x}`));
        const dp = Array(m).fill(0);
        dp[0] = 1; // 시작점
        for (let r = 1; r <= n; r++) {
          for (let c = 1; c <= m; c++) {
            if (r === 1 && c === 1) continue; // 시작점은 이미 1
            if (blocked.has(`${r},${c}`)) {
              dp[c - 1] = 0; // 물웅덩이는 경로 불가
              continue;
            }
            const left = c - 2 >= 0 ? dp[c - 2] : 0;
            const up = dp[c - 1];
            dp[c - 1] = (left + up) % MOD;
          }
        }
        return dp[m - 1];
      }
      ```

   5) 태그 정확성(메타)
      - 본 문제는 본질적으로 DP 문제입니다. 현재 `tags`에 `그리디알고리즘`이 포함되어 있는데, 독자를 위해 정확한 분류(DP/다이나믹프로그래밍)로 정리하는 것을 권장드립니다. (본문 코드는 변경하지 않겠습니다.)

 - 정확성/예외 케이스 체크리스트
   - 첫 행/첫 열에 연속된 물웅덩이가 있는 경우 정상적으로 0이 전파되는지
   - 경로가 전혀 없는 경우 결과가 0인지
   - 큰 입력(m, n 최대)에서도 모듈러가 안정적으로 적용되는지
   - `puddles`가 빈 배열인 경우 기본 DP가 제대로 동작하는지

 - 요약
   - 현재 풀이의 점화식과 로직은 정확하며, 성능도 요구사항에 충분합니다. 가독성과 유지보수성을 위해 모듈러 상수화, 시작점/장애물 처리의 명시화, 1차원 DP 대안 고려를 추천드립니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250905js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42898)

# 🖱️참고 링크

[MDN- has](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set/has)

<br>
[MDN- forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

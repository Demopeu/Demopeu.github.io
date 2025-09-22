---
layout: single
title: "[백준] 21610/마법사 상어와 비바라기/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 시뮬레이션
  - 구현

image:
  path: https://Demopeu.github.io/images/logo/BAEKJOON.png
  alt: "BAEKJOON"
  thumbnail: true
toc: true
author_profile: false
sidebar:
  nav: "counts"
use_math: true
---

![BAEKJOON](https://Demopeu.github.io/images/logo/BAEKJOON.png)

# 💡 문제 설명

마법사 상어는 파이어볼, 토네이도, 파이어스톰, 물복사버그 마법을 할 수 있다. 오늘 새로 배운 마법은 비바라기이다. 비바라기를 시전하면 하늘에 비구름을 만들 수 있다. 오늘은 비바라기를 크기가 N×N인 격자에서 연습하려고 한다. 격자의 각 칸에는 바구니가 하나 있고, 바구니는 칸 전체를 차지한다. 바구니에 저장할 수 있는 물의 양에는 제한이 없다. (r, c)는 격자의 r행 c열에 있는 바구니를 의미하고, A[r][c]는 (r, c)에 있는 바구니에 저장되어 있는 물의 양을 의미한다.

격자의 가장 왼쪽 윗 칸은 (1, 1)이고, 가장 오른쪽 아랫 칸은 (N, N)이다. 마법사 상어는 연습을 위해 1번 행과 N번 행을 연결했고, 1번 열과 N번 열도 연결했다. 즉, N번 행의 아래에는 1번 행이, 1번 행의 위에는 N번 행이 있고, 1번 열의 왼쪽에는 N번 열이, N번 열의 오른쪽에는 1번 열이 있다.

비바라기를 시전하면 (N, 1), (N, 2), (N-1, 1), (N-1, 2)에 비구름이 생긴다. 구름은 칸 전체를 차지한다. 이제 구름에 이동을 M번 명령하려고 한다. i번째 이동 명령은 방향 di과 거리 si로 이루어져 있다. 방향은 총 8개의 방향이 있으며, 8개의 정수로 표현한다. 1부터 순서대로 ←, ↖, ↑, ↗, →, ↘, ↓, ↙ 이다. 이동을 명령하면 다음이 순서대로 진행된다.

1. 모든 구름이 di 방향으로 si칸 이동한다.
2. 각 구름에서 비가 내려 구름이 있는 칸의 바구니에 저장된 물의 양이 1 증가한다.
3. 구름이 모두 사라진다.
4. 물이 증가한 칸 (r, c)에 물복사버그 마법을 시전한다. 물복사버그 마법을 사용하면, 대각선 방향으로 거리가 1인 칸에 물이 있는 바구니의 수만큼 (r, c)에 있는 바구니의 물이 양이 증가한다.
  - 이때는 이동과 다르게 경계를 넘어가는 칸은 대각선 방향으로 거리가 1인 칸이 아니다.
  - 예를 들어, (N, 2)에서 인접한 대각선 칸은 (N-1, 1), (N-1, 3)이고, (N, N)에서 인접한 대각선 칸은 (N-1, N-1)뿐이다.
5. 바구니에 저장된 물의 양이 2 이상인 모든 칸에 구름이 생기고, 물의 양이 2 줄어든다. 이때 구름이 생기는 칸은 3에서 구름이 사라진 칸이 아니어야 한다.
M번의 이동이 모두 끝난 후 바구니에 들어있는 물의 양의 합을 구해보자.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 N, M이 주어진다.

둘째 줄부터 N개의 줄에는 N개의 정수가 주어진다. r번째 행의 c번째 정수는 A[r][c]를 의미한다.

다음 M개의 줄에는 이동의 정보 di, si가 순서대로 한 줄에 하나씩 주어진다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫째 줄에 M번의 이동이 모두 끝난 후 바구니에 들어있는 물의 양의 합을 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 2 ≤ N ≤ 50
- 1 ≤ M ≤ 100
- 0 ≤ A[r][c] ≤ 100
- 1 ≤ di ≤ 8
- 1 ≤ si ≤ 50

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
5 4
0 0 1 0 2
2 3 2 1 0
4 3 2 9 0
1 0 2 9 0
8 8 2 1 0
1 3
3 4
8 1
4 8
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
77
```

# 💡 풀이

## ✍️ 풀이과정

시뮬레이션 문제를 연습 중. 실수하지 않고, 집중한다면 풀 수 있는 문제.

## 📖내가 작성한 JS Code

```javascript
/*
1. 아이디어:
  - 비바라기를 n*n에서
  - 바구니 물 양 제한 없음
  - a에 저장
  - 격자 1,1 시작
  - 그리고 겹침 1이랑 n이랑 %n 하면 될듯
  - 비바라기 시전시 (n,1) , (n,2) (n-1,1), (n-1,2)
  - 이거 m번 명령해서 8방향 이동
  - 물 복사 버그 구현
  2. 시간 복잡도: 해봐야알듯. 일단 메모리는 큼
  */

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

const [n, m, ...array] = input;
const arr = Array.from({ length: n }, (_, idx) =>
  array.slice(idx * n, idx * n + n)
);
const move = array.slice(n ** 2).reduce((acc, _, idx, arr) => {
  if (idx % 2 === 0) acc.push(arr.slice(idx, idx + 2));
  return acc;
}, []);

const dy = [-1, -1, 0, 1, 1, 1, 0, -1];
const dx = [0, -1, -1, -1, 0, 1, 1, 1];

function WaterCopyBugMagic(n, array, water) {
  const dx = [-1, 1, 1, -1];
  const dy = [-1, -1, 1, 1];
  for (const [i, j] of water) {
    let answer = 0;
    for (let l = 0; l < 4; l++) {
      const nx = i + dx[l];
      const ny = j + dy[l];
      if (nx >= 0 && nx < n && ny >= 0 && ny < n && array[nx][ny]) {
        answer++;
      }
    }
    array[i][j] += answer;
  }
}
function MakeCloud(n, array, water) {
  const answer = [];
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      if (array[i][j] > 1 && !water.has(`${i},${j}`)) {
        answer.push([i, j]);
        array[i][j] -= 2;
      }
    }
  }
  return answer;
}

function Sum(array) {
  return array.reduce((acc, cur) => (acc += cur), 0);
}

function solution(n, m, arr, move) {
  let cloud = [
    [n - 1, 0],
    [n - 1, 1],
    [n - 2, 0],
    [n - 2, 1],
  ];
  for ([w, cnt] of move) {
    while (cnt > 0) {
      cnt--;
      for (let idx = 0; idx < cloud.length; idx++) {
        const [x, y] = cloud[idx];
        const nx = x + dx[w - 1];
        const ny = y + dy[w - 1];
        cloud[idx][0] = nx > -1 ? (nx < n ? nx : nx - n) : nx + n;
        cloud[idx][1] = ny > -1 ? (ny < n ? ny : ny - n) : ny + n;
      }
    }
    for ([x, y] of cloud) {
      arr[x][y]++;
    }
    WaterCopyBugMagic(n, arr, cloud);
    cloud = MakeCloud(
      n,
      arr,
      cloud.reduce((acc, cur) => {
        acc.add(`${cur[0]},${cur[1]}`);
        return acc;
      }, new Set())
    );
  }
  return arr.reduce((acc, cur) => (acc += Sum(cur)), 0).toString();
}

process.stdout.write(solution(n, m, arr, move));

```

# 🧠 코드 리뷰

- **접근 방식**: 토러스(가로·세로가 이어진 격자)에서의 구름 이동 → 비 내림 → 구름 소멸 → 물복사버그 → 새로운 구름 생성의 시뮬레이션 파이프라인을 단계별로 정확히 구현했습니다. 좌표는 `arr[x][y]` 형태로 일관되게 사용하고, 방향 매핑과 래핑(wrap-around)을 통해 격자 경계 처리를 해결했습니다.
- **정확성**:
  - 방향 정의: 문제의 1~8 방향(←, ↖, ↑, ↗, →, ↘, ↓, ↙)을 `dx/dy`로 올바르게 매핑했고, 이동 시 `nx = x + dx[w-1]`, `ny = y + dy[w-1]`로 적용되어 방향이 정확합니다.
  - 래핑: 한 칸씩 이동 후 `nx > -1 ? (nx < n ? nx : nx - n) : nx + n` 방식으로 토러스 래핑을 보장합니다. 단, 이는 “한 스텝 이동”을 전제로 합니다.
  - 물복사버그: 대각선 4방만 검사하고, 경계 밖은 제외하며, 물이 있는 칸(`> 0`)만 카운트하여 현재 칸에 누적하는 로직이 문제 조건에 부합합니다.
- **시간/공간 복잡도**: 이동을 `si`번 반복하는 구현이라 대략 `O(M * si * |cloud|)`입니다. 제약(N ≤ 50, M ≤ 100, si ≤ 50)에서는 충분히 빠릅니다. 공간은 격자 `O(N^2)`와 구름·방문표시 수준으로 작습니다.
- **주의할 점(버그 가능성)**:
  - 변수 선언 누락: `for ([w, cnt] of move)`와 `for ([x, y] of cloud)`에서 `const`/`let`이 빠져 전역 변수가 생성됩니다. Node 비엄격 모드에선 동작하긴 하지만 매우 위험합니다. 반드시 `for (const [w, cnt] of move)`/`for (const [x, y] of cloud)`로 선언하세요.
  - 입출력 경로: 백준은 일반적으로 `/dev/stdin`(또는 `0`)을 사용합니다. 현재 `fs.readFileSync("./input.txt")`는 제출 시 실패 원인이 됩니다.
  - 네이밍 혼동: 전역의 `dx/dy`(8방 이동)와 `WaterCopyBugMagic` 내부의 `dx/dy`(대각 이동)가 이름이 겹칩니다. 의미가 다른 벡터이므로 `diagDx/diagDy` 등으로 분리하면 가독성이 좋아집니다.
  - `move` 구성 시 `reduce` 콜백의 4번째 인자 이름이 `arr`라서 바깥의 격자 `arr`와 이름이 충돌합니다. 동작엔 문제 없지만 혼동을 유발하므로 다른 이름(`src` 등)으로 바꾸면 좋습니다.
  - 래핑 로직: 현재는 “1칸씩 si번 이동”이라 안전합니다. 만약 최적화를 위해 한 번에 `si`칸 이동으로 바꾸면, `(x + dx*(si % n) + n) % n`처럼 모듈러 기반으로 구현해야 올바릅니다.
- **개선 제안(리팩터링)**:
  - I/O: `fs.readFileSync(0, "utf8")` 또는 `fs.readFileSync("/dev/stdin", "utf8")`로 교체.
  - 이동 최적화: `const step = si % n;` 후 `nx = (x + dx[w-1] * step + n) % n;`, `ny = (y + dy[w-1] * step + n) % n;`로 한 번에 이동하면 반복 루프를 줄일 수 있습니다.
  - 선언 명확화: 모든 `for...of` 구조 분해에 `const` 사용. 내부 대각선 벡터는 `const diagDx = [...], diagDy = [...]`로 명시.
  - 방문표시: 현재 `MakeCloud` 호출 시 문자열 셋(예: `"i,j"`)을 만들어 전달합니다. 불필요한 문자열 생성 비용을 줄이려면 `visited`를 `boolean[][]`로 만들어 마킹하는 방법도 있습니다(성능 미세 최적화).
  - 의도 드러내기: `Sum`은 한 줄 `reduce`라 인라인 가능하지만, 현재도 충분히 읽기 좋습니다. 다만 `array[nx][ny]`의 진리값 검사 대신 `array[nx][ny] > 0`처럼 의도를 드러내면 더 명확합니다.
- **대안 접근**: 이동·복사·생성 단계를 함수로 잘 분리한 현재 구조가 읽기 좋습니다. 더 나아가 “상태”를 객체로 캡슐화(예: `state = { grid, clouds, visited }`)하면 테스트가 쉬워지고, 각 단계의 순서를 실수로 바꾸는 위험을 줄일 수 있습니다.
- **총평**: 전반적으로 요구사항을 정확히 반영한 깔끔한 시뮬레이션 구현입니다. 실사용(백준 제출)을 위해서는 입력 경로 수정과 `for...of` 변수 선언 보강이 필수이며, 성능 측면에선 `si % n` 기반의 한 번에 이동이 가장 효과적인 개선 포인트입니다. 네이밍 정리까지 하면 가독성과 안정성이 한층 좋아집니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250922js.png)

[백준 문제 보러가기](https://www.acmicpc.net/problem/21610)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

---
layout: single
title: "[백준] 14503/로봇청소기/javascript"
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

로봇 청소기와 방의 상태가 주어졌을 때, 청소하는 영역의 개수를 구하는 프로그램을 작성하시오.

로봇 청소기가 있는 방은 
$N \times M$ 크기의 직사각형으로 나타낼 수 있으며, 
$1 \times 1$ 크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 벽 또는 빈 칸이다. 청소기는 바라보는 방향이 있으며, 이 방향은 동, 서, 남, 북 중 하나이다. 방의 각 칸은 좌표 
$(r, c)$로 나타낼 수 있고, 가장 북쪽 줄의 가장 서쪽 칸의 좌표가 
$(0, 0)$, 가장 남쪽 줄의 가장 동쪽 칸의 좌표가 
$(N-1, M-1)$이다. 즉, 좌표 
$(r, c)$는 북쪽에서 
$(r+1)$번째에 있는 줄의 서쪽에서 
$(c+1)$번째 칸을 가리킨다. 처음에 빈 칸은 전부 청소되지 않은 상태이다.

로봇 청소기는 다음과 같이 작동한다.

1. 현재 칸이 아직 청소되지 않은 경우, 현재 칸을 청소한다.
2. 현재 칸의 주변 $4$칸 중 청소되지 않은 빈 칸이 없는 경우,
  - 바라보는 방향을 유지한 채로 한 칸 후진할 수 있다면 한 칸 후진하고 1번으로 돌아간다.
  - 바라보는 방향의 뒤쪽 칸이 벽이라 후진할 수 없다면 작동을 멈춘다.
3. 현재 칸의 주변 $4$칸 중 청소되지 않은 빈 칸이 있는 경우,
  - 반시계 방향으로 $90^\circ$ 회전한다.
  - 바라보는 방향을 기준으로 앞쪽 칸이 청소되지 않은 빈 칸인 경우 한 칸 전진한다.
  1번으로 돌아간다.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 방의 크기 $N$과 $M$이 입력된다. $(3 \le N, M \le 50)$ 둘째 줄에 처음에 로봇 청소기가 있는 칸의 좌표 $(r, c)$와 처음에 로봇 청소기가 바라보는 방향 $d$가 입력된다. 
$d$가 $0$인 경우 북쪽, $1$인 경우 동쪽, $2$인 경우 남쪽, $3$인 경우 서쪽을 바라보고 있는 것이다.
셋째 줄부터 $N$개의 줄에 각 장소의 상태를 나타내는 $N \times M$개의 값이 한 줄에 $M$개씩 입력된다. 
$i$번째 줄의 $j$번째 값은 칸 $(i, j)$의 상태를 나타내며, 이 값이 $0$인 경우 $(i, j)$가 청소되지 않은 빈 칸이고, $1$인 경우 $(i, j)$에 벽이 있는 것이다. 방의 가장 북쪽, 가장 남쪽, 가장 서쪽, 가장 동쪽 줄 중 하나 이상에 위치한 모든 칸에는 벽이 있다. 로봇 청소기가 있는 칸은 항상 빈 칸이다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

로봇 청소기가 작동을 시작한 후 작동을 멈출 때까지 청소하는 칸의 개수를 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
3 3
1 1 0
1 1 1
1 0 1
1 1 1
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
1;
```

# 💡 풀이

## ✍️ 풀이과정

시뮬레이션의 대표적인 문제. 그냥 문제 그대로 구현하면 되는 문제. 내일 네이버 코테인데 구현력이 필요할 것 같아서 풀어봄.

## 📖내가 작성한 JS Code

```javascript
/*
  1. 아이디어 : 시뮬레이션 문제, 하라는데로 해보자.
  2. 시간복잡도 : 어짜피 n*n임
  3. 자료구조: 그냥 array로 계속 돌아보자.
  */

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

const [n, m, x, y, z, ...matrics] = input;

const dx = [-1, 0, 1, 0];
const dy = [0, 1, 0, -1];

function findCleanZone(n, m, x, y, d, room) {
  let dir = d;
  for (let i = 0; i < 4; i++) {
    dir = (dir + 3) % 4;
    const nx = x + dx[dir];
    const ny = y + dy[dir];
    if (nx >= 0 && nx < n && ny >= 0 && ny < m && room[nx][ny] === 0) {
      return [true, dx[dir], dy[dir], dir];
    }
  }
  return [false];
}

function backMove(n, m, x, y, d, room) {
  const backDir = (d + 2) % 4;
  const bx = x + dx[backDir];
  const by = y + dy[backDir];
  if (bx >= 0 && bx < n && by >= 0 && by < m && room[bx][by] !== 1) {
    return [true, dx[backDir], dy[backDir]];
  }
  return [false];
}

function solution(n, m, first_location, room) {
  let [x, y, d] = first_location;
  let answer = 0;

  while (true) {
    if (room[x][y] === 0) {
      room[x][y] = 2;
      answer++;
    }

    const find_zone = findCleanZone(n, m, x, y, d, room);
    if (find_zone[0]) {
      const [_, mx, my, nd] = find_zone;
      x += mx;
      y += my;
      d = nd;
      continue;
    }

    const back = backMove(n, m, x, y, d, room);
    if (back[0]) {
      x += back[1];
      y += back[2];
    } else {
      return String(answer);
    }
  }
}

process.stdout.write(
  solution(
    n,
    m,
    [x, y, z],
    matrics.reduce(
      (acc, cur) => {
        const node = acc.pop();
        if (node.length < m) {
          node.push(cur);
          acc.push(node);
        } else {
          acc.push(node);
          acc.push([cur]);
        }
        return acc;
      },
      [[]]
    )
  )
);
```

# 🧠 코드 리뷰

- [전반] 시뮬레이션을 문제 설명 그대로 충실히 구현해 깔끔합니다. `findCleanZone()`, `backMove()`, `solution()`으로 책임을 분리한 점이 좋습니다. 시간 복잡도도 최대 O(N·M)으로 적절합니다.

- [입력 파싱] `fs.readFileSync("./input.txt")`는 로컬 테스트에 편하지만, 백준 제출 시에는 보통 `/dev/stdin`을 사용합니다. 블로그 코드 예시라면 현재도 괜찮지만, 실제 제출용에는 `fs.readFileSync(0, "utf8")` 또는 `fs.readFileSync("/dev/stdin", "utf8")`를 권장합니다.
  또한 구조분해 할당에서 `[n, m, x, y, z, ...matrics]`로 받은 뒤 `solution`에서는 `[x, y, d]`로 다시 쓰고 있어, `z`가 `d`(방향)임을 변수명으로 바로 드러내면 가독성이 올라갑니다. 예: `[n, m, r, c, d, ...flat]`.

- [행렬 구성] `matrics.reduce(...)`로 2차원 배열을 만드는 로직은 동작하지만 읽기 난도가 높습니다. 간단한 for 루프나 슬라이싱이 더 직관적입니다.
  예) for 루프로 구성: `const room = Array.from({length: n}, (_, i) => flat.slice(i*m, (i+1)*m));`

- [방향/회전 로직] `dx=[-1,0,1,0]`, `dy=[0,1,0,-1]`로 북/동/남/서 기준이 잘 맞고, `dir=(dir+3)%4`를 4번 수행해 반시계 회전을 탐색하는 방법도 올바릅니다. 다만 `findCleanZone()`이 반환 형태를 배열 플래그로 `[true, dx, dy, dir]`처럼 주는데, 의미가 즉시 보이지 않아 가독성이 떨어집니다. 객체 반환을 고려해 보세요. 예: `{ ok: true, mx, my, nd }`.

- [경계/벽 처리] 전/후진 시 경계와 벽 체크가 정확합니다. 뒤로 이동은 `(d+2)%4`로 계산하며 `room[bx][by] !== 1` 조건을 둬 벽이 아니면 후진하도록 한 점이 명확합니다.

- [상태 값 상수화] `0(미청소)`, `1(벽)`, `2(청소 완료)`를 매직 넘버로 직접 사용하고 있어 의미 파악에 시간이 걸립니다. 상수로 선언하면 유지보수성이 좋아집니다. 예: `const EMPTY=0, WALL=1, CLEANED=2;`

- [부수 효과 관리] `solution()`이 `room`을 직접 변경합니다. 본 문제에선 자연스러운 선택이지만, 재사용 가능성을 고려하면 내부에서 깊은 복사 후 변경하거나, 방문 집합을 별도로 두는 것도 방법입니다.

- [출력 포맷/예제] 실제 코드는 `String(answer)`를 반환하고 `process.stdout.write(...)`로 출력하여 개행이 없습니다. 백준 대부분은 개행 없이도 통과하지만, 안전하게 `\n`을 붙이는 습관을 권장합니다. 또한 본문 예제 출력 블록에 `1;`로 세미콜론이 들어가 있는데, 문제 출력은 정수 `1`입니다. 블로그 문서 예제 출력은 `1`로 표기하는 것이 정확합니다.

- [네이밍/주석] `matrics`(-> `matrix`/`flat`), `find_zone`(스네이크)와 `backMove`(카멜)처럼 네이밍 스타일이 혼재해 있습니다. 하나의 컨벤션으로 통일하면 더 좋습니다. 주석의 맞춤법(`어짜피`→`어차피`, `하라는데로`→`하라는 대로`)도 함께 손보면 문서 완성도가 올라갑니다.

- [테스트 제안] 
  1) 사방이 벽이라 바로 종료되는 케이스
  2) 후진 여러 번이 필요한 복도형 지도
  3) 이미 방문 표시(2) 구역을 재방문하지 않는지 확인하는 케이스
  
  위와 같은 케이스로 빠르게 회귀 테스트를 돌리면 회전/후진 로직 안정성을 더 확신할 수 있습니다.

 # 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250913js.png)

[백준 문제 보러가기](https://www.acmicpc.net/problem/14503)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

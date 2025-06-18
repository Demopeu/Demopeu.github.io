---
layout: single
title: "[Baekjoon] 23293/아주 서바이벌/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
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

때는 2021년, 대한민국에는 '아주 서바이벌'이라는 온라인 게임이 대 유행 중이다. 이 게임은 바다 한가운데의 섬, 아주 아일랜드에서 벌어지는 배틀로얄 게임으로 플레이어들은 아주 아일랜드의 여러 지역을 돌아다니며 아이템을 획득하고, 조합해 다른 플레이어와 싸우게 된다.

상민이는 아주 서바이벌의 서버 개발자다. 이 게임이 흥행하면서 부정행위를 저지르는 플레이어가 늘어나자, 보다 못한 상민이는 게임의 로그를 분석해 부정행위를 전부 찾아내기로 했다.

아주 서바이벌에는 1번부터 53번 지역까지 총 53개의 지역이 존재하며, 모든 플레이어가 1번 지역(정문)에 모인 채로 게임이 시작된다.

플레이어들은 이동, 획득, 조합, 공격 총 네 가지 종류의 행동을 할 수 있다.

- 이동 : 플레이어가 현재 위치한 지역에서 다른 지역으로 이동한다. 즉, 현재 위치한 지역으로는 이동하지 않는다.

- 획득 : 플레이어가 현재 위치한 지역에서만 획득할 수 있는 소재 아이템 1개를 획득한다. 즉, x번 지역에서는 x번 소재 아이템을 획득한다. 아이템의 수량은 충분해 부족할 일이 없으며, 한 플레이어가 같은 아이템을 여러 번 획득할 수 있다.

- 조합 : 플레이어가 가지고 있는 서로 다른 종류의 두 소재 아이템을 1개씩 사용해 장비를 만든다.

- 공격 : 플레이어가 다른 플레이어 한 명을 공격한다. 오직 같은 지역에 있는 플레이어만 공격할 수 있다.

위 행동들에서 상민이는 아래 세 가지 경우를 부정행위라고 판단했다.

1. 플레이어가 현재 위치한 지역에서 얻을 수 없는 소재 아이템을 획득한 경우

2. 플레이어가 가지고 있지 않은 소재 아이템을 사용해 조합하는 경우

3. 플레이어가 다른 지역에 있는 상대 플레이어를 공격하는 경우

상민: 부정행위로 보이는 모든 로그를 기록할 거야. 하지만, 공격할 때 위치를 속이는 것은 참을 수 없어. 그런 플레이어는 차단할 거야!

```
1 11 M 13
2 13 M 15
3 11 F 13
4 11 M 3
5 11 F 3
6 11 C 3 13
7 13 A 11
8 13 F 15
9 13 F 16
10 13 C 15 16
...
```

게임 로그는 "[번호] [플레이어 번호] [행동 코드] [행동 인자]"의 형식으로 기록된다.

- 번호 : 로그의 줄 번호이다. 1번부터 T번까지 순서대로 주어진다.

- 번호 : 로그의 줄 번호이다. 1번부터 T번까지 순서대로 주어진다.

- 행동 코드 : 플레이어가 한 행동이다. 이동은 M(Move), 획득은 F(Farming), 조합은 C(Crafting), 공격은 A(Attack)이다.

- 행동 인자 : 플레이어가 한 행동과 관련된 정보이다. 이동은 새로 이동한 지역의 번호, 획득은 획득한 소재 아이템의 번호, 조합은 조합에 사용된 두 소재 아이템의 번호, 공격은 공격한 플레이어 번호를 행동 인자로 가진다.

부정행위로 획득한 소재 아이템 역시 획득한 것으로 인정되며, 부정행위로 조합 시 가지고 있는 소재 아이템만이 사용된다.

위 로그를 예로 들면, 11번 플레이어는 13번 지역으로 이동하여(1) 13번 소재 아이템을 획득하고(3), 이후 3번 지역으로 이동하여(4) 3번 소재 아이템을 획득해(5) 3번과 13번 소재 아이템을 조합했다(6). 모두 정상적인 행동이다.

13번 플레이어는 15번 지역으로 이동한 후(2), 3번 지역에 있는 11번 플레이어를 공격했다(7). 다른 지역에 있는 플레이어를 공격하는 것은 부정행위이기 때문에 7번 로그를 기록하고, 공격 부정행위이기 때문에 13번 플레이어는 차단해야 한다. 이어서, 15번 소재 아이템을 획득하고(8), 16번 소재 아이템을 획득 후에(9), 15번과 16번 소재 아이템을 조합했다(10). 15번 지역에서 16번 소재 아이템을 획득하는 것은 부정행위이기 때문에 9번 로그를 기록한다. 하지만, 16번 소재 아이템을 획득한 것은 인정되기 때문에 10번 로그는 부정행위가 아니다.

상민이를 위해 게임의 로그를 분석하고, 기록된 부정행위와 차단할 플레이어를 상민에게 알려주자.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫 번째 줄에는 게임 로그의 줄 수 T, 플레이어 수 N이 주어진다. (1 ≤ T ≤ 200,000, 1 ≤ N ≤ 100,000)

두 번째 줄부터 T개 줄 동안 게임 로그가 입력된다. 각 줄의 게임 로그는 번호, 플레이어 번호, 행동 코드, 행동 인자가 공백 한 칸을 사이에 두고 주어진다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫 번째 줄에 부정행위로 기록된 로그의 수를 출력한다. 기록된 로그가 없다면 "0"을 출력한다.

부정행위로 기록된 로그가 있다면 다음 줄에 기록된 로그의 번호를 공백 한 칸씩 띄어서 오름차순으로 출력한다. 기록된 로그가 없다면 해당 줄은 출력하지 않는다.

다음 줄에 차단할 플레이어 수를 출력한다. 차단할 플레이어가 없다면 "0"을 출력한다.

차단할 플레이어가 있다면 다음 줄에는 차단할 플레이어의 번호를 공백 한 칸씩 띄어서 오름차순으로 출력한다. 한 플레이어가 여러 번 부정행위를 저지르더라도 한 번만 출력하며, 차단할 플레이어가 없다면 해당 줄은 출력하지 않는다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
7 20
1 11 M 13
2 13 M 15
3 11 F 13
4 11 M 3
5 11 F 3
6 11 C 3 13
7 13 A 11
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
1
7
1
13
```

# 💡 풀이

## ✍️ 풀이과정

완전 구현 문제. 그런데 몇번을 틀렸음. 이유는 난 로그에 적히면 바로 벤 되는 줄 알았는데 실행이 되야했었음. 그리고 다 작성하고 정답 맞추고 보니 53번 구역까지 있어야하는데, 그거 체크 안했는데 통과함. 너무 예외가 많은데 구현이라 이걸 하나하나 다 적어서 했어야했음.
그리고 Map이랑 includes, indexOf를 너무 오랜만에 사용해서 공부를 좀 했음.

## 📖 내가 작성한 Python Code

```python

```

## 📖내가 작성한 JS Code

```javascript
const fs = require("fs");
const inputs = fs.readFileSync("./input.txt").toString().trim().split`\n`;

const [T, N] = inputs[0].trim().split(" ").map(Number);
let log_count = 0;
const log_list = [];
const player_blocked = new Set();
const bag = new Map();

for (let i = 1; i <= N; i++) {
  bag.set(i, [1, []]);
}

for (let i = 1; i <= T; i++) {
  const log = inputs[i].trim().split(" ");
  const logNum = i;
  const player = Number(log[1]);
  const code = log[2];

  if (player < 1 || player > N) {
    log_list.push(logNum);
    log_count++;
    continue;
  }

  const [pos, items] = bag.get(player);

  if (code === "M") {
    const newPos = Number(log[3]);
    bag.set(player, [newPos, items]);
  } else if (code === "F") {
    const item = Number(log[3]);
    if (item !== pos) {
      log_list.push(logNum);
      log_count++;
    }
    items.push(item);
    bag.set(player, [pos, items]);
  } else if (code === "C") {
    const a = Number(log[3]);
    const b = Number(log[4]);

    if (a === b) {
      log_list.push(logNum);
      log_count++;
      continue;
    }

    let aIndex = items.indexOf(a);
    let bIndex = items.indexOf(b);

    if (aIndex === -1 || bIndex === -1) {
      log_list.push(logNum);
      log_count++;
    }

    if (aIndex !== -1) {
      items.splice(aIndex, 1);
    }
    bIndex = items.indexOf(b);
    if (bIndex !== -1) {
      items.splice(bIndex, 1);
    }

    bag.set(player, [pos, items]);
  } else if (code === "A") {
    const target = Number(log[3]);

    if (!bag.has(target)) {
      log_list.push(logNum);
      log_count++;
      player_blocked.add(player);
      continue;
    }

    const [targetPos] = bag.get(target);
    if (targetPos !== pos) {
      log_list.push(logNum);
      log_count++;
      player_blocked.add(player);
    }
  } else {
    log_list.push(logNum);
    log_count++;
  }
}

console.log(log_count);
if (log_count > 0) console.log(log_list.join(" "));
console.log(player_blocked.size);
if (player_blocked.size > 0) {
  console.log([...player_blocked].sort((a, b) => a - b).join(" "));
}
```

---

# 🧠 코드 리뷰

## 1. 개선 포인트: 자기 위치로 이동 시 부정행위 처리

```javascript
if (code === "M") {
  const newPos = Number(log[3]);
  bag.set(player, [newPos, items]);
}
```

현재 위치인지 아닌지 체크 없이 바로 이동시킴 → ❌ 부정행위 검출 누락

**개선 방향**

```javascript
if (code === "M") {
  const newPos = Number(log[3]);
  if (newPos === pos) {
    log_list.push(logNum);
    log_count++;
    continue;
  }
  bag.set(player, [newPos, items]);
}
```

---

# 💻결과

<strong style="font-size: 1.2em">🐍 python</strong>

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-06-18/j_result.PNG)

[백준문제 보러가기](https://www.acmicpc.net/problem/23293)

---

# 🖱️참고 링크

[MDN web docs js includes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

[MDN web docs js indexOf](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

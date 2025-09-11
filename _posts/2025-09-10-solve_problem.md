---
layout: single
title: "[Baekjoon] 백트래킹 special"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 백트래킹

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

오늘은 카카오 코테 준비를 위해 백트래킹 8문항을 빠르게 풀어보았다.

# 💡 1번 문제 : N과 M (1)

[문제 설명](https://www.acmicpc.net/problem/15649)


## 💡 풀이

### ✍️ 풀이과정

백트래킹으로 했는데, 착각했던거, 어짜피 1,2,3 순서대로 드가는건데 처음에 풀때는 sort()를 한적이 있다. 필요 없으니 기초적인 백트래킹으로 바로 풀면 됨.

### 📖내가 작성한 JS Code

```javascript
/*
1. 아이디어 : 각 수열을 백트래킹으로 다뽑고 그걸 sort 치고 다시 string으로 어떰?
2. 시간복잡도 : mCn(8!) +  nlogn 할만함
3. 자료구조: 그냥 range(m)을 넣으면 될듯
*/

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

function solution(m, n) {
  const visited = Array(m + 1).fill(false);
  const answer = [];
  const dfs = (acc, cur) => {
    if (cur === n) {
      answer.push(acc.join(" "));
      return;
    }
    for (let i = 1; i <= m; i++) {
      if (!visited[i]) {
        visited[i] = true;
        acc.push(i);
        dfs(acc, cur + 1);
        acc.pop();
        visited[i] = false;
      }
    }
  };
  dfs([], 0);
  return answer.join("\n");
}

process.stdout.write(solution(input[0], input[1]));
```

### 🧠 코드 리뷰

불필요한 정렬 없이 표준 백트래킹으로 중복 없는 길이 n의 순열을 정확히 열거하며 시간·공간 복잡도도 적절

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js1.png)

# 💡 2번 문제 : N과 M (2)

[문제 설명](https://www.acmicpc.net/problem/15650)


## 💡 풀이

### ✍️ 풀이과정

무조건 숫자 커야하니까 위에서 for문의 크기 변경만하면 되고, 중복은 없으니까 visited 필요 없음.

### 📖내가 작성한 JS Code

```javascript
/*
1. 아이디어 : 각 수열을 백트래킹으로 다뽑고 무조건 커야하니까 for만 조절
2. 시간복잡도 : mCn 보다 작은듯
3. 자료구조: 그냥 range(m)을 넣으면 될듯
*/

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

function solution(m, n) {
  const answer = [];
  const dfs = (acc, cur) => {
    if (cur === n) {
      answer.push(acc.join(" "));
      return;
    }
    for (let i = acc.length ? acc[acc.length - 1] + 1 : 1; i <= m; i++) {
      acc.push(i);
      dfs(acc, cur + 1);
      acc.pop();
    }
  };
  dfs([], 0);
  return answer.join("\n");
}

process.stdout.write(solution(input[0], input[1]));

```

### 🧠 코드 리뷰
증가하는 시작값으로 가지치기해 조합을 중복 없이 사전순으로 생성하며 불필요한 방문/정렬이 없어 효율적입니다.

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js2.png)

# 💡 3번 문제 : N과 M (3)

[문제 설명](https://www.acmicpc.net/problem/15651)


## 💡 풀이

### ✍️ 풀이과정

이번엔 더 쉬움. 그냥 for문 1부터 하면 됨.

### 📖내가 작성한 JS Code

```javascript
/*
1. 아이디어 : 이번엔 중복이 다 됨. 더 쉬움. 1부터 하면 됨.
2. 시간복잡도 : m^n
3. 자료구조: 그냥 range(m)을 넣으면 될듯
*/

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

function solution(m, n) {
  const answer = [];
  const dfs = (acc, cur) => {
    if (cur === n) {
      answer.push(acc.join(" "));
      return;
    }
    for (let i = 1; i <= m; i++) {
      acc.push(i);
      dfs(acc, cur + 1);
      acc.pop();
    }
  };
  dfs([], 0);
  return answer.join("\n");
}

process.stdout.write(solution(input[0], input[1]));

```

### 🧠 코드 리뷰
중복 허용을 전제로 깊이 n까지 전개하는 완전탐색으로 모든 수열을 누락 없이 생성하며 구조가 단순해 빠르게 구현 가능합니다.

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js3.png)

# 💡 4번 문제 : N과 M (4)

[문제 설명](https://www.acmicpc.net/problem/15652)


## 💡 풀이

### ✍️ 풀이과정

이번엔 비내림차순이여야한다고한다. 그냥 2,3번 합치면 됨.

### 📖내가 작성한 JS Code

```javascript
/*
1. 아이디어 : 원래 비내림차순 아님?이 아니라 3 2 3 이런게 있었네
2. 시간복잡도 : m^n
3. 자료구조: 그냥 range(m)을 넣으면 될듯
*/

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);

function solution(m, n) {
  const answer = [];
  const dfs = (acc, cur) => {
    if (cur === n) {
      answer.push(acc.join(" "));
      return;
    }
    for (let i = acc.length ? acc[acc.length - 1] : 1; i <= m; i++) {
      acc.push(i);
      dfs(acc, cur + 1);
      acc.pop();
    }
  };
  dfs([], 0);
  return answer.join("\n");
}

process.stdout.write(solution(input[0], input[1]));
```

### 🧠 코드 리뷰

비내림차순 조건을 시작값 고정으로 자연스럽게 강제해 중복과 역순을 제거한 채 모든 경우를 효율적으로 탐색합니다.

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js4.png)

# 💡 5번 문제 : N-Queen

[문제 설명](https://www.acmicpc.net/problem/9663)

## 💡 풀이

### ✍️ 풀이과정

모르면 틀리는 문제. 한 행에서 열에 대하여 돌면 됨. 대각선이 문제인데, 항상 대각선은 r-c와 r+c가 같음.
```
r-c = 2-3 = -1 이 일정한 칸들: (0,1), (1,2), (2,3), (3,4)
      c=0 c=1 c=2 c=3 c=4
r=0    .   •   .   .   .
r=1    .   .   •   .   .
r=2    .   .   .   ★   .
r=3    .   .   .   .   •
r=4    .   .   .   .   .
r+c = 2+3 = 5 이 일정한 칸들: (0,4), (1,3), (2,2), (3,1)
      c=0 c=1 c=2 c=3 c=4
r=0    .   .   .   .   .
r=1    .   .   .   .   •
r=2    .   .   .   ★   .
r=3    .   .   •   .   .
r=4    .   •   .   .   .
```
이거 보면 좀 이해할듯. 이번엔 비트마스킹으로 도전하려고 했는데, 실패해서 그냥 단순히 이렇게 풀었음. 다시 도전하게 적어둠.

### 📖내가 작성한 JS Code

```javascript
  /*
  1. 아이디어 : 한 행 당 하나. 그래서 열에 대하여만 하면 됨. 그 전 열이랑 대각선만 확인하면 됨.
  2. 시간복잡도 : N^2
  3. 자료구조: Set써서 해시로 빨리 찾자.
  */

  const fs = require("fs");
  const input = Number(fs.readFileSync("./input.txt").toString().trim());

  function solution(n) {
    let answer = 0;
    const cols = new Set();
    const d1 = new Set();
    const d2 = new Set();

    const dfs = (r) => {
      if (r === n) {
        answer++;
        return;
      }
      for (let c = 0; c < n; c++) {
        if (cols.has(c) || d1.has(r + c) || d2.has(r - c)) continue;
        cols.add(c);
        d1.add(r + c);
        d2.add(r - c);
        dfs(r + 1);
        cols.delete(c);
        d1.delete(r + c);
        d2.delete(r - c);
      }
    };
    dfs(0);
    return answer;
  }

  process.stdout.write(String(solution(input)));
```

### 🧠 코드 리뷰

열과 두 대각선을 Set으로 O(1) 검증해 가지치기를 극대화한 정석적인 백트래킹 구현입니다.

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js5.png)


# 💡 6번 문제 : 연산자 끼워넣기

[문제 설명](https://www.acmicpc.net/problem/14888)


## 💡 풀이

### ✍️ 풀이과정

완전탐색하면 될꺼라고 직감적으로 옴. N이 막 크지 않았기 때문에, 역시 통과.

### 📖내가 작성한 JS Code

```javascript
/*
  1. 아이디어 : 이거 백트래킹이 가능한가? 나는 dfs로 보임.
  순서대로 하니까 그냥 숫자 기록하고 나눗셈 조심하면 될듯
  2. 시간복잡도 : 4^(n-1) 정도 아닌가? 4의 10승 2초 가능할듯?
  3. 자료구조:array
  */

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);
const [n, ...nokori] = input;
const array = nokori.slice(0, n);
const count = nokori.slice(n);

function solution(n, array, count) {
  let answer = [-Infinity, Infinity];

  const divid = (a, b) => {
    if (a >= 0) return ~~(a / b);
    return -~~(-a / b);
  };

  const dfs = (sum, cnt) => {
    if (cnt === n) {
      answer[0] = Math.max(sum, answer[0]);
      answer[1] = Math.min(sum, answer[1]);
      return;
    }
    for (let i = 0; i < 4; i++) {
      if (count[i]) {
        switch (i) {
          case 0:
            count[i]--;
            dfs(sum + array[cnt], cnt + 1);
            count[i]++;
            break;
          case 1:
            count[i]--;
            dfs(sum - array[cnt], cnt + 1);
            count[i]++;
            break;
          case 2:
            count[i]--;
            dfs(sum * array[cnt], cnt + 1);
            count[i]++;
            break;
          case 3:
            count[i]--;
            dfs(divid(sum, array[cnt]), cnt + 1);
            count[i]++;
            break;
          default:
            console.log("로직이 이상해요");
        }
      }
    }
  };
  dfs(array[0], 1);

  return answer.join("\n");
}

process.stdout.write(solution(n, array, count));
```

### 🧠 코드 리뷰

연산자 잔여 개수로 분기를 제한한 DFS로 모든 수식을 탐색하며 음수 나눗셈을 규칙대로 처리해 최댓·최솟값을 정확히 갱신합니다.

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js7.png)



# 💡 7번 문제 : 스타트와 링크

[문제 설명](https://www.acmicpc.net/problem/14889)


## 💡 풀이

### ✍️ 풀이과정



### 📖내가 작성한 JS Code

```javascript
/*
  1. 아이디어 : 또 안보인다 백트래킹. 숫자 돌면서 다 더한 값에 절반보다 작아지면 걍 끝내면 될듯?
  2. 시간복잡도 : N이 20이라 할만한듯
  3. 자료구조:array
  */

const fs = require("fs");
const input = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split(/\s+/)
  .map(Number);
const [n, ...array] = input;
const s = array.reduce(
  (acc, cur) => {
    let node = acc.pop();
    if (node.length < n) {
      node.push(cur);
      acc.push(node);
    } else {
      acc.push(node);
      acc.push([cur]);
    }
    return acc;
  },
  [[]]
);

function solution(n, s) {
  const visited = Array(n).fill(false);
  let answer = Infinity;
  const dfs = (count, idx) => {
    if (count === n / 2) {
      let myTeam = 0;
      let otherTeam = 0;
      for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
          if (visited[i] && visited[j]) myTeam += s[i][j];
          if (!visited[i] && !visited[j]) otherTeam += s[i][j];
        }
      }
      answer = Math.min(answer, Math.abs(myTeam - otherTeam));
      return;
    }
    for (let i = idx; i < n; i++) {
      if (!visited[i]) {
        visited[i] = true;
        dfs(count + 1, i);
        visited[i] = false;
      }
    }
  };
  dfs(0, 0);
  return answer.toString();
}

process.stdout.write(solution(n, s));

```

### 🧠 코드 리뷰

조합 DFS로 N/2 팀을 구성한 뒤 시너지 합을 계산해 두 팀 차이를 갱신하며 시작 인덱스로 중복 탐색을 제거해 효율을 확보했습니다.

### 💻결과

![js_result](https://Demopeu.github.io/images/result/20250910js8.png)


# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

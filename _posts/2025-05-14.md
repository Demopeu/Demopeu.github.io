---
layout: single
title: "[Baekjoon] 1679/숫자놀이/python/javascript"
categories: 문제풀이
tags:
  - python
  - javascript
  - 백준
  - 알고리즘
  - DP
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

홀순이(holsoon)와 짝순이(jjaksoon) 둘이서 숫자 게임을 한다.

예를 들어, 정수 1과 3이 주어지고, 이 둘을 통틀어 5번까지 마음대로 사용하여 그 합을 구하여 1,2,3,…을 만드는 놀이다.

이 경우 먼저 홀순이가 1 하나만을 사용하여 1을 만든다. 짝순이는 1+1로 1을 두 번 사용하여 2를 만들고, 다시 홀순이는 3을 만들어야하는데 1+1+1로 1을 세 번 사용하거나 3을 한 번 사용하여 3을 만든다.

짝순이는 1+1+1+1, 1+3으로 4를 만든다. 서로 번갈아서 상대방의 수보다 1이 큰 수를 만들어야 한다. 단, 1과 3을 통틀어 최대 5번 사용한다.

이런 식으로 진행하면 13까지는 만들 수 있지만 14를 만들지 못하게 되므로 짝순이가 졌다.

숫자 게임에서 사용하는 정수 N개와 최대 사용 횟수 K가 주어질 때, 누가 어느 수에서 이기는지를 판별하는 프로그램을 작성해보자

사용하는 정수에는 반드시 1이 포함된다. 그렇지 않으면 홀순이가 1을 만들지 못하므로 무조건 지게 된다.

1이 꼭 있으니 상대방이 만든 방법에 1만 한 번 더 쓰면 된다고 생각하기 쉽지만, 최대 사용 횟수가 정해져 있으므로, 이 방법이 수가 커지는 경우에는 잘 되지 않는다.

위에서 13을 홀순이가 만들었지만 짝순이는 최대 사용 횟수 때문에 14를 만들지 못하고 진다.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 숫자 게임에서 사용하는 정수의 수 N이, 둘째 줄에는 사용하는 정수가 크기 순으로 주어진다. 셋째 줄에는 최대 사용 횟수 K가 주어진다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫째 줄에 누가 몇 번째 수에서 이겼는지를 출력한다. 예제에서는 짝순이가 14를 못 만들어서, 홀순이가 14에서 이겼다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 1 ≤ N ≤ 1,000
- 1 ≤ K ≤ 50
- 숫자 게임에서 사용하는 정수는 1000보다 작거나 같은 자연수이고, 중복되는 수가 주어지지 않는다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
2
1 3
5
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
holsoon win at 14
```

# 💡 풀이

## ✍️ 풀이과정

보자마자, DP를 떠올릴 수 있었다. 한 때, DP만 열심히 푼 보람이 있는듯. 작은 결과로 큰 결과를 만든다.

## 📖 내가 작성한 Python Code

```python
import sys

def dp(lst,k):
    dp_list = [k+1]*(max_number:= max(lst)*k+1)
    dp_list[0] = 0
    for number in range(1,max_number):
        for i in lst:
            if (idx:=number-i) < 0:
                break
            dp_list[number] = min(dp_list[number], dp_list[idx] + 1)
        if dp_list[number] > k:
            return 'jjaksoon' if number % 2 else 'holsoon', str(number)


def main():
    inputs = sys.stdin.read().split()
    use_number_list = [int(inputs[i+1]) for i in range(int(inputs[0]))]
    winner, win_number = dp(use_number_list,int(inputs[-1]))
    sys.stdout.write(f"{winner} win at {win_number}")

if __name__ == '__main__':
    main()
```

## 📖내가 작성한 JS Code

```javascript
const fs = require("fs");
const inputs = fs
  .readFileSync("input.txt") // or "/dev/stdin"
  .toString()
  .trim()
  .split(/\s+/);

const dp = (list, k) => {
  const max_number = Math.max(...list) * k + 1;
  const dp_list = new Array(max_number).fill(k + 1);
  dp_list[0] = 0;
  for (let i = 1; i < max_number; i++) {
    for (const j of list) {
      const idx = i - j;
      if (idx < 0) {
        break;
      }
      dp_list[i] = Math.min(dp_list[i], dp_list[idx] + 1);
    }
    if (dp_list[i] > k) {
      return [i % 2 ? "jjaksoon" : "holsoon", i];
    }
  }
};

const problem = (inputs) => {
  const number = Number(inputs[0]);
  const use_number_list = Array.from({ length: number }, (_, idx) =>
    Number(inputs[idx + 1])
  );
  return dp(use_number_list, Number(inputs[number + 1]));
};
const [winner, win_number] = problem(inputs);
console.log(`${winner} win at ${win_number}`);
```

---

# 🧠 코드 리뷰

## 1. `dp_list` 최대 길이 설정

```python
dp_list = [k + 1] * (max(lst) * k + 1)
```

**문제점**

- `max(lst) * k`는 이론상 만들 수 있는 최대 수이지만, 실제로는 **처음 만들 수 없는 수**를 찾으면 바로 게임이 종료됨.
- 따라서 이렇게 큰 DP 테이블은 **불필요한 메모리 낭비**로 이어질 수 있음.

**개선 방향**

- 수를 1부터 차례로 만들면서, 만들 수 없는 순간을 발견하면 즉시 종료하면 됨.
- 예시:

```python
for number in range(1, max_possible + 1):
    ...
    if dp_list[number] > k:
        break
```

- 또는 BFS 탐색으로 필요한 만큼만 공간을 사용하는 방식도 고려할 수 있음.

## 2. `number - i < 0: break` 구문

```python
for i in lst:
    if (idx := number - i) < 0:
        break
```

**문제점**

- 리스트가 **정렬되어 있다는 전제**가 있어야 `break`가 유효함.
- 일반적으로는 `continue`가 더 안전하지만, 본 문제에선 조건이 명확히 주어져 있어 문제가 되진 않음.

**개선 방향**

- 가독성을 위해 `break` 대신 다음처럼 명시하는 방식도 고려 가능:

```python
if number - i >= 0:
    dp_list[number] = min(dp_list[number], dp_list[number - i] + 1)
```

## 3. 승자 판별 로직

```python
return 'jjaksoon' if number % 2 else 'holsoon'
```

**문제점**

- `number % 2`는 현재 차례를 판별하는 방식이지만, 의미가 **암묵적**이어서 오해 소지가 있음.
- 예: 짝수면 홀순이 차례, 홀수면 짝순이 차례 → 코드에서 명시적으로 표현되지 않음.

**개선 방향**

- 가독성을 높이기 위해 변수 또는 주석 처리 추가:

```python
is_holsoon_turn = number % 2 == 0
winner = 'holsoon' if is_holsoon_turn else 'jjaksoon'
```

## 4. 불필요한 문자열 변환

```python
return ..., str(number)
```

**문제점**

- 숫자를 문자열로 바꾸는 것은 **출력용으로만 필요**하며, 내부 로직에는 부적합함.

**개선 방향**

- 숫자는 **그대로 유지**하고 출력 시 포맷에만 사용:

```python
return winner, number
```

## 5. 입력 처리 방식

```python
inputs = sys.stdin.read().split()
```

**문제점**

- 전체 입력을 한 줄로 받아 분해하는 방식은 **명확성과 유지보수성**이 떨어짐.

**개선 방향**

- `readline()`을 활용해 명시적으로 각 줄을 파싱하는 것이 더 명확함:

```python
N = int(sys.stdin.readline())
numbers = list(map(int, sys.stdin.readline().split()))
K = int(sys.stdin.readline())
```

---

# 💻결과

<strong style="font-size: 1.2em">🐍 python</strong>

![python_result](https://Demopeu.github.io/images/2025-05-14/p_result.PNG)

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-05-14/j_result.PNG)

[백준문제 보러가기](https://www.acmicpc.net/problem/1679)

---

# 🖱️참고 링크

[DP 예시용 링크](https://wikidocs.net/206429)

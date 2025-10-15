---
layout: single
title: "[Programmers] 43238/입국심사/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 이분탐색

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/43238)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n    | times    |
| -------- | -------- |
| 6    | [7, 10]    |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 28    |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.
- 각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.
- 심사관은 1명 이상 100,000명 이하입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
n: 6;
times: [7, 10];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
28;
```

# 💡 풀이

## ✍️ 풀이과정

정말 단순한 이분탐색 문제. 그런데 계속 시간 초과가 나는거임. 왜일까 싶었다. 그래서 reduce를 조기에 끊어야하나 했다가. Math.floor를 사용해보니 통과.
난 보통 ~~((mid/cur) /2) 이런식으로 사용함. 비트 연산이기 때문에 빠르기 때문이다.

gpt 한테 물어보니 이건 처리 방식의 차이라고 한다.
비트 연산 강제로 32비트 부호 정수로 변환. 이때 범위 초과가 나서 wrap 발생. 루프가 수백만 번 더 반복 됬다고 한다.

Math.floor는 배정밀도 부동소수로 64비트로 변환. 따라서 정확도 유지가 된다고한다.

js는 몫을 하는 //가 주석이기 때문에 여러가지 방법이 있다.
1. ~~
2. parseInt
3. Math.floor
4. Math.trunc

각자 찾아보는게 좋은데 역시 비트 연산이 가장 빠르고, parseInt가 가장 느리다고 한다.

어렵다. 어려워.

## 📖내가 작성한 JS Code

```javascript
function solution(n, times) {
    let [start, end] = [1, Math.min(...times) * n];

    while (start <= end) {
        const mid = Math.floor((start + end) / 2);
        let people = 0;

        for (let i = 0; i < times.length; i++) {
            people += Math.floor(mid / times[i]);
            if (people >= n) break; // early break
        }

        if (people >= n) end = mid - 1;
        else start = mid + 1;
    }
    return start;
}
```

# 🧠 코드 리뷰

- 이분 탐색 경계/종료: `while (start<=end)` 후 `return start`로 하한을 반환하는 로직이 정확합니다.
- 상한 설정: `end = Math.max(...times) * n`은 유효하지만 불필요하게 큽니다. `end = Math.min(...times) * n`이 더 타이트해 탐색 횟수를 줄일 수 있습니다.
- 정수 안전성: 최악의 경우 `end`가 1e18에 달해 Number 정밀도(2^53-1)를 초과할 수 있습니다. JS에선 `BigInt`로 변환해 계산하는 것이 안전합니다.
- 미세 최적화: `reduce` 대신 for 루프가 미세하게 빠를 수 있으나 현재도 충분합니다. 가독성 우선이면 그대로 유지해도 좋습니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20251013js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/43238)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
<br>
[MDN- Math.floor](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
<br>
[MDN- parseInt](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt)


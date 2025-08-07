---
layout: single
title: "[Baekjoon] 29730/임스의 데일리 인증 스터디/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 문자열
  - 정렬
  - 파싱
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

취업 준비생 임스는 취업 준비를 하면서 그날그날 무슨 공부를 하였는지 기록하기 위해 데일리 인증이라는 스터디를 시작했다. 임스는 매일 무슨 공부를 하였는지 적으면서 몇 개의 규칙을 정했다.

- 매일 꾸준히 백준 문제를 푼다.
- 백준 문제를 하루 $1$문제 이상 풀었고, 그 외의 다른 공부는 $0$개 이상 진행하였다. 다른 공부들은 영어 대소문자, 숫자, 공백으로만 이루어진 최대 길이 $100$의 문자열이다.
- 인증 기록으로는 백준 문제 링크를 제일 마지막에 작성하고, 그 외 학습 기록은 문자열 길이가 짧은 순으로 정렬해서 작성한다. 만약 문자열의 길이가 같다면, 사전 순으로 정렬한다.
- 문자는 아스키코드 기준으로 비교한다.
- 백준 문제 링크는 boj.kr/문제 번호 형식이다. 문제 번호가 작은 순서대로 정렬해서 작성한다. 문제 번호는 $1$ 이상 $30\,000$ 이하이다.
임스가 하루 동안 공부한 기록들이 정렬되지 않은 채로 주어졌을 때, 주어진 규칙에 맞게 정렬 후 출력한다.
<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫 번째 줄에는 임스가 하루동안 공부한 기록의 개수 
$N$이 주어진다. $(1 \le N \le 1\,000)$
다음 $N$개의 줄에 임스가 하루동안 공부한 기록들이 한 줄에 하나씩 주어진다.

학습 기록은 공백으로 시작하거나 끝나지 않는다. 같은 공부 기록이 여러 번 주어질 수도 있다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

임스가 공부한 기록들을 주어진 규칙에 맞게 정렬 후 한 줄에 하나씩 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
3
boj.kr/1307
study gc
read book
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
study gc
read book
boj.kr/1307
```

# 💡 풀이

## ✍️ 풀이과정

왜 쉽다! 했는데 정말정말 오래걸렸다. 이 문제에서 중복이 있다길래 처음에 중복을 제거해버렸고, 아스키코드가 아니라 유니코드로 정리했었다. 그래서 왜 틀렸지 머리를 싸맸다.
그런데 1~30000까지였는데 이건 검사안해도 통과되는거 보고 좀 신기했음.
문제 좀 똑바로 읽어야겠다.

## 📖내가 작성한 JS Code

```javascript
const fs = require("fs");
const input = fs.readFileSync(0).toString().trim().split("\n");
let bojUrls = [];
const notInBaekjoonInput = input
  .slice(1)
  .map((el) => {
    if (el.length > 6 && el.slice(0, 7) === "boj.kr/") {
      const num = Number(el.slice(7));
      if (!isNaN(num)) {
        bojUrls.push(el);
        return null;
      }
    }
    return el;
  })
  .filter(Boolean)
  .sort((a, b) => {
    if (a.length !== b.length) {
      return a.length - b.length;
    }
    if (a < b) return -1;
    if (a > b) return 1;
    return 0;
  });
if (notInBaekjoonInput.length > 0) {
  console.log(notInBaekjoonInput.join("\n"));
}
console.log(
  bojUrls.sort((a, b) => Number(a.slice(7)) - Number(b.slice(7))).join("\n")
);

```

# 🧠 코드 리뷰



# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-08-07/j_result.png)

[백준문제 보러가기](https://www.acmicpc.net/problem/29730)

# 🖱️참고 링크

[javascript Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)

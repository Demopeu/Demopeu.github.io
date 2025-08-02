---
layout: single
title: "[Baekjoon] 3216/다운로드/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 그리디알고리즘
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

택희는 인터넷에서 노래를 다운받으려고 한다. 노래는 여러 조각으로 나누어져 있고, 정해진 순서대로 다운받아야 한다. 택희는 각 조각의 노래 길이와 다운로드 길이를 알고 있다.

택희는 노래를 모두 다운받기 전에 들으려고 한다. 음악이 중간에 끊여지면 분위기를 망치기 때문에, 한 번 듣기 시작하면 노래는 멈추지 않고 끝까지 재생해야 한다. 각 조각을 들으려면 그 조각을 모두 다운로드 해야 들을 수 있다.

택희가 음악을 끊김없이 들으려면, 다운로드 시작한지 몇 초 후에 들으면, 끊김 없이 노래를 들을 수 있는지 구하는 프로그램을 작성하시오.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 조각의 수 N이 주어진다. (1 ≤ N ≤ 100,000)

다음 N개의 줄에는 노래의 길이 D와 다운로드하는데 걸리는 시간 V가 주어진다. (1 ≤ D,V ≤ 1000)

<strong style="font-size: 1.5em"> 📤 출력</strong>

첫째 줄에, 다운로드 시작하고 몇 초 후에 노래를 듣기 시작하면, 끊김 없이 들을 수 있는지 출력한다. 그러한 시간이 여러개라면, 가장 빠른 것을 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
4
2 1
1 5
3 3
2 4
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
7
```

# 💡 풀이

## ✍️ 풀이과정

다음주에 코테 시험이 2개나 있는데 빈둥거리다가 서둘러서 푼 문제. 이게 실버 1? 일까 하는 수준의 간단한 구현문제라고 생각했다. 남은 시간과 시작 시간을 계산을 하면 되기 때문.

## 📖내가 작성한 JS Code

```javascript
const fs = require("fs");
const input = fs.readFileSync("./input.txt").toString().trim().split(/\s+/);

let [startTime, nokoriTime] = [0, 0];

for (let i = 1; i <= Number(input[0] * 2); i += 2) {
  songLength = Number(input[i]);
  downloadTime = Number(input[i + 1]);
  if (nokoriTime < downloadTime) {
    startTime += downloadTime - nokoriTime;
    nokoriTime = songLength;
  } else {
    nokoriTime = nokoriTime - downloadTime + songLength;
  }
}

console.log(startTime);
```

---

# 🧠 코드 리뷰

작성하신 코드는 그리디 알고리즘의 핵심을 잘 파악하여 문제를 정확하게 해결하고 있습니다.
startTime과 nokoriTime이라는 두 변수를 통해, 음악이 끊기지 않기 위해 필요한 최소 대기  
 시간을 효율적으로 계산하는 로직이 돋보입니다.

전반적으로 훌륭한 풀이지만, 코드의 가독성과 안정성을 더욱 높이기 위해 몇 가지 개선점을  
 제안해 드립니다.

1. 변수 선언

for 루프 내에서 songLength와 downloadTime 변수를 let이나 const 키워드 없이 사용하셨습니다.
이 경우, 이 변수들은 의도치 않게 전역 변수로 선언됩니다. 이는 코드의 다른 부분에서 예기치
않은 오류를 발생시킬 수 있습니다. const 키워드를 사용하여 변수를 선언하는 것이 좋습니다.

1 // 수정 제안
2 for (let i = 1; i <= Number(input[0] \* 2); i += 2) {
3 const songLength = Number(input[i]);
4 const downloadTime = Number(input[i + 1]);
5 // ... 이하 로직
6 }

2. 변수명

nokoriTime이라는 변수명은 일본어와 영어가 혼합되어 있습니다. 의미를 이해하는 데 문제는
없지만, playBuffer 또는 remainingPlayTime과 같이 영어로 통일된 명확한 변수명을 사용하면
다른 사람이 코드를 읽거나 미래에 코드를 수정할 때 더 이해하기 쉽습니다.

3. 입력 데이터 처리

현재 코드는 입력을 하나의 긴 배열로 처리하고 for 루프의 인덱스를 이용해 각 데이터에
접근하고 있습니다. 이 방식은 간단한 문제에서는 효과적이지만, 입력이 더 복잡해지면 코드를  
 이해하고 유지보수하기 어려워질 수 있습니다.

입력 처리 로직을 문제 풀이 로직과 분리하면 코드가 더 명확해집니다. 예를 들어, 입력을 먼저 {
songLength, downloadTime } 형태의 객체 배열로 변환한 후, 이 배열을 순회하며 문제를 푸는  
 방식입니다.

    1 // 입력 처리 분리 예시
    2 const fs = require("fs");
    3 const input = fs.readFileSync("./input.txt").().trim().split('\n');
    4 const N = Number(input[0]);                  t
    5 const songs = [];                            o
    6 for (let i = 1; i <= N; i++) {               S
    7   const [D, V] = input[i].split(' ').map(Number);
    8   songs.push({ songLength: D, downloadTime: V });
    9 }                                            i

10 n
11 let startTime = 0; g
12 let playBuffer = 0;
13
14 for (const song of songs) {
15 if (playBuffer < song.downloadTime) {
16 startTime += song.downloadTime - playBuffer;
17 playBuffer = song.songLength;
18 } else {
19 playBuffer = playBuffer - song.downloadTime + song.songLength;
20 }
21 }
22
23 console.log(startTime);

총평

문제에 대한 핵심적인 접근과 논리 전개는 매우 훌륭합니다. 위에서 제안한 몇 가지 스타일과  
 구조적인 개선을 적용한다면, 더욱 안정적이고 가독성 높은 코드가 될 것입니다. 수고하셨습니다

---

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-08-02/j_result.png)

[백준문제 보러가기](https://www.acmicpc.net/problem/3216)

---

# 🖱️참고 링크

[그리디 알고리즘 위키백과](https://ko.wikipedia.org/wiki/%ED%83%90%EC%9A%95_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

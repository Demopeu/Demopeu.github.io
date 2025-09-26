---
layout: single
title: "[백준] 2733/Brainf*ck/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 시뮬레이션
  - 구현
  - 자료 구조
  - 문자열
  - 스택

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

Brainf*ck은 Urban Müller가 1993년에 만든 프로그래밍 언어이다. 그의 목적은 역사상 가장 작은 튜링 완전 언어(Turing -complete language)의 컴파일러를 만드는 것이었다.위키백과에는 다음과 같은 설명이 적혀져 있다. (designed to challenge and amuse programmers, and was not made to be suitable for practical use)

이 언어는 0으로 초기화 된 크기가 32768바이트인 바이트 배열, 배열의 맨 첫 바이트를 가리키는 포인터를 가지고 있다.

다음과 같이 7가지 명령어를 가지고 있으며, 각 명령어는 문자 1글자이다. (원래 8가지 명령어를 가지고 있지만, 문제를 위해 하나를 지웠다)

- >: 포인터를 증가시킨다. 만약, 포인터 값이 32767이면 0이된다.
- <: 포인터를 감소시킨다. 만약, 포인터 값이 0이면 32767이 된다.
- +: 포인터가 가리키는 값을 증가시킨다. 255를 증가시키면 0이 된다.
- -: 포인터가 가리키는 값을 감소시킨다. 0을 감소시키면 255가 된다.
- .: 포인터가 가리키는 값을 ASCII문자로 출력한다.
- [: 포인터가 가리키는 값이 0이면, 짝이 되는 뒤쪽의 ]로 이동한다.
- ]: 포인터가 가리키는 값이 0이 아니면, 짝이되는 앞쪽의 [로 이동한다.
Brainf*ck 프로그램이 주어졌을 때, 이 프로그램의 출력을 출력하는 프로그램을 작성하시오.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫째 줄에 프로그램의 개수 T(1 ≤ T ≤ 100)가 주어진다. 각 프로그램은 한줄 또는 그 이상으로 구성되어 있으며, end만 적혀있는 줄로 끝난다. 프로그램에 올바르지 않은 문자 (<>+-.[])가 있다면, 이는 무시하고 넘어가야 한다. %는 주석을 의미하며, %가 나온 뒤에 나오는 해당 줄의 문자는 모두 무시한다. 프로그램의 최대 명령어 개수는 128,000이다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

각 프로그램의 결과를 다음과 같이 출력한다. 첫째 줄에 PROGRAM #n을 출력한다. n은 프로그램 번호이다. (첫 번째 프로그램부터 차례대로 1이고, 1 ≤ n ≤ N이다). 둘째 줄에는 프로그램의 결과를 출력한다. 만약 [나 ]가 짝이 맞지 않을 대는 COMPILE ERROR를 출력하면 된다. 출력에서 여러 줄을 출력할 수도 있다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
3
++++++++[>+++++++++ % hello-world.
<-]>.<+++++[>++++++<-]>-.+++++++..
+++.<++++++++[>>++++<<-]>>.<<++++[>
------<-]>.<++++[>++++++<-]>.+++.
------.--------.>+.
end
+++[>+++++++[.
end
%% Print alphabet, A-Z.
+ + + + + +++++++++++++++++++++>
++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++
+< [ >.+<- ]
end
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
PROGRAM #1:
Hello World!
PROGRAM #2:
COMPILE ERROR
PROGRAM #3:
ABCDEFGHIJKLMNOPQRSTUVWXYZ
```

# 💡 풀이

## ✍️ 풀이과정

그냥 한문제 풀고 공부할려고 했는데 잘못 물렸음. 왜이리 기냐 문제가. 그냥 커멘드를 구현하면 되는데 비트로 할 생각은 처음에는 안했음. 그런데, 찾아보면서 Uint8Array,fromCharCode 이거는 생각도 못함. 이런 문제는 안나오겠지? 그냥 시뮬레이션 능력 기르려다가 너무 많은 시간을 잡아먹어 버림.

## 📖내가 작성한 JS Code

```javascript


const fs = require("fs");
let [testcase, ...input] = fs
  .readFileSync("./input.txt")
  .toString()
  .trim()
  .split("\n");

const MAX_SIZE = 32768;

function applyCommand(memory, pointer, cmd, out) {
  switch (cmd) {
    case ">":
      return (pointer + 1) & (MAX_SIZE - 1);
    case "<":
      return (pointer - 1) & (MAX_SIZE - 1);
    case "+":
      memory[pointer] = (memory[pointer] + 1) & 0xff;
      return pointer;
    case "-":
      memory[pointer] = (memory[pointer] - 1) & 0xff;
      return pointer;
    case ".":
      out.push(String.fromCharCode(memory[pointer]));
      return pointer;
    default:
      return pointer;
  }
}

function findMap(program, pair) {
  const stack = [];
  for (let i = 0; i < program.length; i++) {
    const c = program[i];
    if (c === "[") stack.push(i);
    else if (c === "]") {
      if (stack.length === 0) return false;
      const j = stack.pop();
      pair.set(i, j);
      pair.set(j, i);
    }
  }
  if (stack.length) return false;
  return true;
}

function solution(testcase, input, MAX_SIZE) {
  let NUMBER = Number(testcase);
  let check = 1;
  const cli = input.slice();
  const output = [];

  while (check <= NUMBER) {
    output.push(`PROGRAM #${check}:`);
    const programLines = [];
    while (true) {
      const line = cli.shift();
      if (line === "end") break;
      for (const command of line) {
        if (command === "%") break;
        if ("><+-[].".includes(command)) programLines.push(command);
      }
    }
    const pair = new Map();
    const flag = findMap(programLines, pair);
    if (!flag) {
      output.push("COMPILE ERROR");
      check++;
      continue;
    }

    const memory = new Uint8Array(MAX_SIZE);
    let pointer = 0;
    let ip = 0;
    const out = [];

    while (ip < programLines.length) {
      const cmd = programLines[ip];
      if (cmd === "[") {
        if (memory[pointer] === 0) {
          ip = pair.get(ip) + 1;
        } else {
          ip += 1;
        }
        continue;
      }
      if (cmd === "]") {
        if (memory[pointer] !== 0) {
          ip = pair.get(ip);
        } else {
          ip += 1;
        }
        continue;
      }
      pointer = applyCommand(memory, pointer, cmd, out);
      ip += 1;
    }
    output.push(out.join(""));
    check++;
  }
  return output.join("\n");
}

console.log(solution(testcase, input, MAX_SIZE));

```

# 🧠 코드 리뷰


# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250926js.png)

[백준 문제 보러가기](https://www.acmicpc.net/problem/2733)

# 🖱️참고 링크

[MDN- switch](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/switch)

<br/>

[MDN- Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)

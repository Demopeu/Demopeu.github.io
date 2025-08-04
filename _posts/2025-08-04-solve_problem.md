---
layout: single
title: "[Baekjoon] 33848/퍼시스턴트 스택/javascript"
categories: 문제풀이
tags:
  - javascript
  - 백준
  - 알고리즘
  - 자료구조
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

퍼시스턴트를 아세요?

어떤 자료구조가 "퍼시스턴트(persistent)하다"는 것은 현재까지 자료의 상태 변화를 모두 보존하고 있다는 것이다. 이 문제에서 여러분들은 퍼시스턴트 스택을 구현해야 한다. 아래와 같은 쿼리를 수행하는 프로그램을 작성하시오.

$1$: 스택의 가장 위에 값 $i$를 집어넣는다.

$2$: 스택의 가장 위에 있는 값을 제거한다. 스택이 비어 있지 않은 경우에만 주어진다.

$3$ $j$: 최근 $j$개의 $1$번 또는 $2$번 쿼리를 취소한다. 취소할 수 있는 $1$번 또는 $2$번 쿼리가 $j$개 이상인 경우에만 주어진다.

$4$: 스택의 크기를 출력한다.

$5$: 스택의 가장 위에 있는 값을 출력한다. 만약 스택이 비어 있다면 대신 -1을 출력한다.

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

첫 번째 줄에 쿼리의 개수를 나타내는 정수 $Q$가 주어진다. ($1 \leq Q \leq 200\, 000$)

두 번째 줄부터 $Q$개의 줄에 걸쳐 한 줄에 하나씩 쿼리가 주어진다. ($1 \leq i \leq 10^9;1 \leq j \leq Q$)

$4$번 또는 $5$번 쿼리는 한 번 이상 주어진다. 주어지는 모든 수는 정수이다.

<strong style="font-size: 1.5em"> 📤 출력</strong>

$4$번 또는 $5$번 쿼리가 주어질 때마다 쿼리의 답을 출력한다.

<strong style="font-size: 1.5em">📏 제한 사항</strong>

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
15
1 1
1 2
4
5
1 3
4
5
2
4
5
3 1
4
5
3 3
4
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
2
2
3
3
2
2
3
3
0
```

# 💡 풀이

## ✍️ 풀이과정

js는 클래스 객체 지향형 언어가 아니다. 프로토타입 언어인데 사용자 편의를 위해서 문법이 있음. 평소에서 python으로 풀었을때 클래스를 사용하였기 때문에 이렇게 풀어봄. 그런데 시간초과가 떴음. 
시간 초과가 날만한 곳이 없는데 하고 혹시나해서 출력을 모아 한번만 하니까 통과됨.

그런데 아무리 생각해도 이상했음. 그래서 node.js의 console.log에 대하여 찾아봄

[console.log()가 느린 이유](https://medium.com/@xiaweiliang94/why-you-should-think-twice-before-using-console-log-and-tips-for-avoiding-performance-pitfalls-1228efc27360)

요약하면,
1. console.log()는 성능에 크게 영향을 미칠 수 있다. 이는 매 호출마다 객체 직렬화와 출력을 위한 문자열 생성을 수행하기 때문이다.

2. 메모리 사용량을 크게 증가시킨다.

라는 것이다. 좀 더 개인적으로 찾아보니

1. 싱글 스레드 + 이벤트 루프에서 console.log는 동기적으로 실행되어 이벤트 루프를 막아버림

2. TTY에 즉시 flush함.

3. V8엔진의 최적화 방해 이 변수가 콘솔에 출력될 수 있다고 가정하고 최적화 피함.

이거에 대하여 조만간 다시 글을 작성할 예정이다.

그리고 이런 간단한 문제는 class화 하지 말고 그냥 풀어도 됬을듯?

## 📖내가 작성한 JS Code

```javascript
const fs = require("fs");
const input = fs.readFileSync("./input.txt").toString().trim().split("\n");

class persistentStack {
  constructor() {
    this.stack = [];
    this.queryStack = [];
    this.output = [];
  }
  queryPush(number) {
    this.stack.push(number);
    this.queryStack.push([1, number]);
  }
  queryPop() {
    const number = this.stack.pop();
    this.queryStack.push([2, number]);
  }
  reverseQuery(number) {
    for (let i = 0; i < number; i++) {
      const query = this.queryStack.pop();
      if (query[0] === 1) {
        this.stack.pop();
      } else {
        this.stack.push(query[1]);
      }
    }
  }
  queryLength() {
    this.output.push(this.stack.length);
  }
  queryTop() {
    const queryLength = this.stack.length;
    if (queryLength > 0) {
      this.output.push(this.stack[queryLength - 1]);
    } else {
      this.output.push(-1);
    }
  }
}
const newpersistentStack = new persistentStack();

for (let i = 1; i <= Number(input[0]); i++) {
  const query = input[i].split(" ");
  switch (query[0]) {
    case "1":
      newpersistentStack.queryPush(Number(query[1]));
      break;
    case "2":
      newpersistentStack.queryPop(Number(query[1]));
      break;
    case "3":
      newpersistentStack.reverseQuery(Number(query[1]));
      break;
    case "4":
      newpersistentStack.queryLength();
      break;
    case "5":
      newpersistentStack.queryTop();
      break;
  }
}
console.log(newpersistentStack.output.join("\n"));

```

# 🧠 코드 리뷰

1. 🔍 queryPop()의 매개변수 제거
```javascript
// 기존 코드
queryPop(number) { ... }

// 개선 (number 파라미터는 실제로 안 씀)
queryPop() { ... }
```
→ 이건 오타에 가까운 부분이라 의미는 없지만 정리하면 더 깔끔함.

2. 📦 queryTop()에서 this.stack.at(-1) 사용 가능
```javascript
queryTop() {
  console.log(this.stack.at(-1) ?? -1);
}
```
this.stack.length > 0 체크 대신, Array.prototype.at(-1)를 쓰면 코드가 더 직관적

단, 구형 Node.js (v16 이하)에서는 at()이 없으니 환경에 따라 사용 여부 결정

3. 🧪 reverseQuery의 안정성 향상 (에러 방지용)
```javascript
reverseQuery(number) {
  while (number-- > 0 && this.queryStack.length > 0) {
    const query = this.queryStack.pop();
    ...
  }
}
```
문제 조건상 j는 항상 충분한 수의 쿼리를 취소한다고 되어 있지만,

테스트 시 실수로 잘못된 입력을 넣는다면 에러가 날 수 있으므로 while 조건을 더 안전하게 작성 가능

4. 🧼 네이밍 개선 제안 (선택적)

|현재 이름|제안 이름|이유
|---|---|---
queryPush|pushQuery or push|일반적인 스택 연산 명명 방식과 통일성
queryStack|historyStack|의미를 더 명확하게 전달

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/2025-08-04/j_result.png)

[백준문제 보러가기](https://www.acmicpc.net/problem/33848)

# 🖱️참고 링크

[console.log()가 느린 이유](https://medium.com/@xiaweiliang94/why-you-should-think-twice-before-using-console-log-and-tips-for-avoiding-performance-pitfalls-1228efc27360)

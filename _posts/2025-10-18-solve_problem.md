---
layout: single
title: "[Programmers] 42627/디스크 컨트롤러/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 힙

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42627)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| jobs                     |
| ------------------------ |
| [[0, 3], [1, 9], [2, 6]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 8      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 1 ≤ jobs의 길이 ≤ 500
- jobs[i]는 i번 작업에 대한 정보이고 [s, l] 형태입니다.
  - s는 작업이 요청되는 시점이며 0 ≤ s ≤ 1,000입니다.
  - l은 작업의 소요시간이며 1 ≤ l ≤ 1,000입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
jobs: [
  [0, 3],
  [1, 9],
  [2, 6],
];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
8;
```

# 💡 풀이

## ✍️ 풀이과정

간단하게 힙 구현한 다음, jobs은 안건들고 하려고 idx를 이용함.

## 📖내가 작성한 JS Code

```javascript
class MinHeap {
  constructor(compare) {
    this.heap = [];
    this.compare = compare;
  }
  _p(i) {
    return (i - 1) >> 1;
  }
  _l(i) {
    return i * 2 + 1;
  }
  _r(i) {
    return i * 2 + 2;
  }

  bubbleUp(i) {
    while (i > 0) {
      const p = this._p(i);
      if (this.compare(this.heap[p], this.heap[i]) <= 0) break;
      [this.heap[p], this.heap[i]] = [this.heap[i], this.heap[p]];
      i = p;
    }
  }
  bubbleDown() {
    let i = 0;
    const n = this.heap.length;
    while (true) {
      const l = this._l(i);
      const r = this._r(i);
      let s = i;
      if (l < n && this.compare(this.heap[l], this.heap[s]) < 0) s = l;
      if (r < n && this.compare(this.heap[r], this.heap[s]) < 0) s = r;
      if (s === i) break;
      [this.heap[s], this.heap[i]] = [this.heap[i], this.heap[s]];
      i = s;
    }
  }
  heapqpush(value) {
    this.heap.push(value);
    this.bubbleUp(this.heap.length - 1);
  }
  heapqpop() {
    const n = this.heap.length;
    if (n === 0) return undefined;
    if (n === 1) return this.heap.pop();
    const top = this.heap[0];
    this.heap[0] = this.heap.pop();
    this.bubbleDown();
    return top;
  }
  heapqsize() {
    return this.heap.length;
  }
}

function solution(jobs) {
  jobs.sort((a, b) => a[0] - b[0]);

  const heap = new MinHeap((a, b) => a[1] - b[1]);
  const n = jobs.length;

  let [time, idx, done, total] = [0, 0, 0, 0];

  while (done < n) {
    while (idx < n && jobs[idx][0] <= time) {
      heap.heapqpush(jobs[idx]);
      idx++;
    }

    if (heap.heapqsize() > 0) {
      const [arr, dur] = heap.heapqpop();
      time += dur;
      total += time - arr;
      done++;
    } else {
      time = jobs[idx][0];
    }
  }

  return ~~(total / n);
}
```

# 🧠 코드 리뷰

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20251018js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42627)

# 🖱️참고 링크

[MDN- class](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)
<br>
[MDN- sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

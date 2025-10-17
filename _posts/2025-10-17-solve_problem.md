---
layout: single
title: "[Programmers] 42626/더 맵게/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| scoville | K |
| -------- | ----- |
| [1, 2, 3, 9, 10, 12] | 7 |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 2  |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
scoville: [1, 2, 3, 9, 10, 12];
K: 7;
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
2;
```

# 💡 풀이

## ✍️ 풀이과정

파이썬에서 js로 갈아타고 난다음 제일 열받는게 내가 구현해야한다는 점임. 그래서 좀 외워보려고 함.

## 📖내가 작성한 JS Code

```javascript
class MinHeap{
    constructor(compare = (a, b) => a - b){
        this.heap = [];
        this.compare = compare;
    }
    /** index helper */
    getParentsIdx(i){return (i-1) >> 1;}
    getLeftIdx(i){return i*2 + 1;}
    getRightIdx(i){return i*2 + 2;}
    
    /** engine */
    bubbleUp(i){
        while(i>0){
            const p = this.getParentsIdx(i);
            if (this.compare(this.heap[p], this.heap[i]) <= 0) break;
            [this.heap[p], this.heap[i]] = [this.heap[i], this.heap[p]];
            i = p;
        }
    }
    bubbleDown(){
        let i = 0;
        const n = this.heap.length;
        while(true){
            const l = this.getLeftIdx(i);
            const r = this.getRightIdx(i);
            let s = i;
            if (l < n && this.compare(this.heap[l], this.heap[s]) < 0) s = l;
            if (r < n && this.compare(this.heap[r], this.heap[s]) < 0) s = r;
            if (s === i) break;
            [this.heap[s],this.heap[i]] = [this.heap[i],this.heap[s]];
            i = s;
        }
        
    }
    /** methods */
    push(value){
        this.heap.push(value);
        this.bubbleUp(this.heap.length - 1);
    }
    pop(){
        const n = this.heap.length;
        if (n === 0) return undefined;
        else if (n === 1) return this.heap.pop();
        const top = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.bubbleDown();
        return top;
    }
    peek() { return this.heap[0]; }
    size() { return this.heap.length; }
    isEmpty() { return this.heap.length === 0; }
}

function solution(scoville, K) {
    const heap = new MinHeap();
    scoville.forEach(e=>heap.push(e));
    let cnt = 0;
    while (heap.peek() < K){
        if (heap.size() < 2) return -1;
        cnt++;
        const newScoville = heap.pop()+heap.pop()*2;
        heap.push(newScoville);
    }
    return cnt;
}
```

# 🧠 코드 리뷰
- **정확성**
  - `MinHeap` 구현은 기본 연산(`push/pop/peek/size`)과 상향/하향 힙 정렬이 올바르게 작동하도록 작성됨.
  - `solution()`에서 최소값이 `K` 이상이 될 때까지 두 원소를 섞는 반복 로직이 문제 요구사항과 일치.
  - 힙이 2개 미만일 때 `-1` 반환하여 불가능 케이스 처리 OK.

- **시간/공간 복잡도**
  - 현재는 `scoville.forEach(e => heap.push(e))`로 O(n log n)에 초기화. 이후 각 섞기 연산이 O(log n)이라 총 O(n log n) 내외로 적절.
  - 공간은 힙 O(n).

- **엣지 케이스**
  - `K <= 0`이면 즉시 0을 반환해도 됨. 현재 구현도 while 조건이 거짓이어서 0을 반환하지만, 명시적 가드를 두면 가독성이 향상됨.
  - 모든 값이 0이고 `K > 0`인 경우 반복적으로 섞다가 원소가 1개 남으면 `-1` 반환하는 흐름이 정상 동작.

- **개선 제안(가독성/성능)**
  - 힙 초기화 성능 개선: O(n) `heapify` 지원 시 대량 초기화가 더 빠름. 이를 위해 `bubbleDown(i)`처럼 인덱스를 인자로 받는 하향 함수가 필요함. 현재 `bubbleDown()`은 루트 전용.
  - 네이밍: `getParentsIdx` → `getParentIdx`가 더 자연스러움.
  - 가드 추가 제안:
    - 입력 검증(선택): `if (!Array.isArray(scoville)) throw ...`
    - 조기 종료: `if (K <= 0) return 0;`
  - 미세 최적화: `while (heap.size() >= 2 && heap.peek() < K)`로 루프 진입 조건을 더 안전하게 표현 가능.

- **대안 구현 힌트(요약)**
  - `bubbleDown(i)` 형태로 일반화하고, 생성자에서 배열을 받아 `for (let i = Math.floor(n/2)-1; i>=0; i--) bubbleDown(i)`로 O(n) 힙 빌드.
  - 로직 자체는 현재로도 충분히 통과 가능하며, 위 개선은 성능/가독성 향상 목적.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20251017js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

# 🖱️참고 링크

[MDN- forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

<br>

[MDN- class](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)



---
layout: single
title: "[Programmers] 86491/최소직사각형/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 자료구조
  - 구현
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/86491)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| sizes                   |
| ----------------------- |
| [[60, 50], [30, 70], [60, 30], [80, 40]] |
| [[10, 7], [12, 3], [8, 15], [14, 7], [5, 15]] |
| [[14, 4], [19, 6], [6, 16], [18, 7], [7, 11]]|

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 4000    |
| 120    |
| 133    |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- sizes의 길이는 1 이상 10,000 이하입니다.
  - sizes의 원소는 [w, h] 형식입니다.
  - w는 명함의 가로 길이를 나타냅니다.
  - h는 명함의 세로 길이를 나타냅니다.
  - w와 h는 1 이상 1,000 이하인 자연수입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
sizes: [[60, 50], [30, 70], [60, 30], [80, 40]]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
4000
```

# 💡 풀이

## ✍️ 풀이과정

핵심 아이디어는 각 명함을 긴 변과 짧은 변으로 정규화한 뒤, 긴 변의 최대값과 짧은 변의 최대값을 곱하는 것입니다. 초기 풀이에서는 `reduce`와 `Array.prototype.sort`를 사용해 간결하게 구현했습니다.

## 📖내가 작성한 JS Code

```javascript
function solution(sizes) {
    return sizes.reduce((acc,cur)=>{
        cur.sort((a,b)=>b-a);
        acc[0] = Math.max(acc[0],cur[0]);
        acc[1] = Math.max(acc[1],cur[1]); 
        return acc
        },[0,0]).reduce((acc,cur)=>acc*cur,1);
}
```

# 🧠 코드 리뷰

- __입력 변이 방지__: `cur.sort(...)`는 원본 배열을 변이합니다. 문제 풀이에는 영향이 없지만, 함수형 스타일이나 재사용성을 고려하면 입력을 변이하지 않는 편이 안전합니다.
- __불필요한 정렬 비용__: 두 원소만 비교하면 되므로 정렬 대신 단순 비교로 더 명확하게 표현할 수 있습니다.
- __가독성__: 이중 `reduce`는 한 번에 읽기 어렵습니다. 누적값을 이름 있는 변수로 두거나, 한 번의 순회로 최대값을 갱신한 뒤 최종 곱을 반환하면 의도가 더 분명합니다.
- __시간·공간 복잡도__: 모두 O(n). 다만 정렬 호출을 제거하면 상수 시간 오버헤드가 줄어듭니다.

### ✅ 개선된 JS 코드 (비변이·단일 순회)

```javascript
function solution(sizes) {
  let maxW = 0;
  let maxH = 0;
  for (const [w, h] of sizes) {
    const big = w >= h ? w : h;
    const small = w >= h ? h : w;
    if (big > maxW) maxW = big;
    if (small > maxH) maxH = small;
  }
  return maxW * maxH;
}
```

동일한 아이디어를 보다 명확하게 드러내며, 입력 배열을 변이하지 않습니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250822js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/86491)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

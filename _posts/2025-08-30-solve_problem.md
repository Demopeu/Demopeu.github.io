---
layout: single
title: "[Programmers] 84512/모음 사전/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 완전탐색
  - 정수론

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| word |
| ------- |
| "AAAAE"   |
| "AAAE" |
| "I" |
| "EIO" |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 6      |
| 10      |
| 1563      |
| 1189      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- word의 길이는 1 이상 5 이하입니다.
- word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
word: "AAAAE"
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
6
```

# 💡 풀이

## ✍️ 풀이과정

완전탐색이라고 되어 있는 문제. 하지만, 효율이 너무 않좋음. 문제 의도는 모든 순서를 다 만들어서 만약 중간에 목표 워드가 나오면 백트래킹하는 문제일듯. 하지만, 그냥 수학적으로 하면 안되나? 해서 해봄.

## 📖내가 작성한 JS Code

```javascript
function solution(words){
    return words.split('').reduce((acc,cur,idx)=>acc+[781,156,31,6,1][idx] * ['A','E','I','O','U'].indexOf(cur) + 1,0) 
}
```

# 🧠 코드 리뷰


# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250830js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

# 🖱️참고 링크

[MDN- reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

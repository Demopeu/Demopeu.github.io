---
layout: single
title: "[Programmers] 42584/주식가격/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 자료구조
  - 정렬
  - stack
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

프로그래머스라 문제 설명은 링크로 대체

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42584)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| prices          |
| --------------- |
| [1, 2, 3, 2, 3] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result          |
| --------------- |
| [4, 3, 1, 1, 0] |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[1, 2, 3, 2, 3]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
[4, 3, 1, 1, 0]
```

# 💡 풀이

## ✍️ 풀이과정

그냥 평범하게 스텍으로 푸는 문제. 보통 이런 문제는 for 2중으로 쓰면 터지도록 설계하는데 여기서도 똑같음.

## 📖내가 작성한 JS Code

```javascript
function solution(prices) {
  const n = prices.length;
  const stack = [];
  const answer = Array(n).fill(0);
  for (let i = 0; i < n; i++) {
    while (stack.length && prices[stack[stack.length - 1]] > prices[i]) {
      const j = stack.pop();
      answer[j] = i - j;
    }
    stack.push(i);
  }
  while (stack.length) {
    const j = stack.pop();
    answer[j] = n - 1 - j;
  }
  return answer;
}
```

# 🧠 코드 리뷰

- **정확성**: 단조 스택으로 각 인덱스의 최초 하락 시점을 기록하고, 남은 인덱스는 끝까지 유지 시간으로 계산해 결과가 정확합니다.
- **복잡도**: 각 인덱스가 스택에 최대 1회 push/pop → 시간 O(n), 공간 O(n).
- **동일 가격 처리**: `>` 비교로 동일 가격은 하락으로 보지 않아 끝까지 유지 시간으로 자연스럽게 처리됩니다.
- **가독성**: `stack`이 “인덱스 스택”임을 주석으로 한 줄 명시하고, `j`는 `idx` 등으로 이름을 명확히 하면 이해가 쉬워집니다.
- **안전성**: 입력 제약상 n≥2이지만, 방어적으로 빈 배열 시 `[]` 반환 처리 추가를 고려할 수 있습니다.
- **대안/미세개선**: 현재 구현이 최적(O(n))입니다. 불필요한 마이크로 최적화는 지양하고 주석/네이밍만 보강하면 충분합니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250820js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42584)

# 🖱️참고 링크

[MDN- for(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for)<br>
[MDN- while(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/while)

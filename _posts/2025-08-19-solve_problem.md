---
layout: single
title: "[Programmers] 12909/올바른 괄호/javascript"
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

프로그래머스라 문제 설명은 링크로 대체

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| s        |
| -------- |
| "()()"   |
| "(()())" |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| true   |
| true   |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
"()()"
"(()())"
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
true
true
```

# 💡 풀이

## ✍️ 풀이과정

딱히 어려운 점은 없었는데 String을 어떻게 하나하나 처리할까 하다가 그냥 언팩해봄. 그런데 됨. 사실 이거 보다는 바로 종료하는게 더 효율적이지만, reduce로 함.

## 📖내가 작성한 JS Code

```javascript
function solution(s) {
  return [...s].reduce((acc, cur) => {
    if (acc === -1) {
      return acc;
    } else if (cur === "(") {
      acc += 1;
    } else if (acc > 0) {
      acc -= 1;
    } else {
      return -1;
    }
    return acc;
  }, 0) === 0
    ? true
    : false;
}
```

# 🧠 코드 리뷰

- **동작 요약**

  - `reduce`로 누적 카운트를 계산하며, 불가능 상태를 `-1` 센티넬로 고정해 조기 실패를 표현합니다. 최종 누적값이 0이면 `true`를 반환합니다.

- **좋은 점**

  - 시간 복잡도 O(n)으로 요구사항 충족.
  - 불가능 상태를 빠르게 전파해 추가 연산을 줄이려는 의도가 좋습니다.

- **개선 사항**

  - 문자열을 배열로 언팩(`[...]`)하고 `reduce`를 쓰면 불필요한 메모리(O(n))가 발생합니다. 단순 카운터 + 조기 반환이 더 직관적이고 효율적입니다.
  - 마지막 반환의 삼항 연산자(`=== 0 ? true : false`)는 불필요합니다. 불 표현식 그대로 반환하세요.
  - 센티넬 `-1` 패턴은 의도 파악에 시간이 걸릴 수 있어 가독성이 떨어집니다.

- **권장 구현(가독성/성능 우선)**

  ```javascript
  function solution(s) {
    let count = 0;
    for (const ch of s) {
      if (ch === "(") count++;
      else {
        if (count === 0) return false; // 닫는 괄호가 더 많음
        count--;
      }
    }
    return count === 0;
  }
  ```

- **복잡도**

  - 시간: O(n)
  - 공간: O(1) (권장안), O(n) (현재 `spread + reduce`)

- **기타 문서 관련(선택 반영 권장)**
  - 제한 사항이 배열 기준으로 작성되어 있어 문제(12909)와 불일치합니다. `s` 길이(≤100,000), 문자 집합 `'(' , ')'`로 교정 권장.
  - 하단 링크가 12906으로 연결됩니다. 12909로 수정 권장.
  - front matter 이미지와 본문 첫 이미지가 중복 노출됩니다. 한쪽만 유지하세요.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250819js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

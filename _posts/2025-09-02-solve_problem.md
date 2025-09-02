---
layout: single
title: "[Programmers] 42883/큰 수 만들기/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 그리디알고리즘

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42883)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| number | k  |
| --- | ----- |
| "1924"  | 2 |
| "1231234" | 3 |
| "4177252841" | 4 |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| "94"      |
| "3234"      |
| "7752841"      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- number는 2자리 이상, 1,000,000자리 이하인 숫자입니다.
- k는 1 이상 number의 자릿수 미만인 자연수입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
number: "1924"
k: 2
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
"94"
```

# 💡 풀이

## ✍️ 풀이과정

스텍을 활용하면 되는 문제. 그런데 생각보다 힘들었다. 나중에 한번 다시 풀어봐야겠다. forEach문이 이쁘게 나오지 못함. ai한테 주니까 
```javascript
function solution(number, k) {
  const answer = [];
  number.split("").forEach((e) => {
    while (k > 0 && answer.length && answer.at(-1) < e) {
      answer.pop();
      k--;
    }
    answer.push(e);
    // 여기서 return은 없어도 똑같음
  });
  return answer.slice(0, number.length - k).join("");
}
```
이러는데 이게 더 이쁜듯.

## 📖내가 작성한 JS Code

```javascript
function solution(number, k) {
    const answer = [];
    number.split("").forEach((e,idx)=>{
        let stackLength = answer.length
        while (stackLength && k >0){
            if (answer[stackLength-1]<e){
                stackLength--;
                k--;
                answer.pop();
            } else break;
        }
        answer.push(e);
        return
    })
    return answer.slice(0, number.length - k).join("");
}
```

# 🧠 코드 리뷰

# 개요
- __접근__: 스택 기반 그리디로 직전 원소보다 큰 수가 오면 pop하며 `k`를 소진. 전형적이고 정답 로직입니다.
- __복잡도__: 최악 O(n). 각 문자는 최대 1회 push/pop.

# 코드 품질 피드백
- __불필요한 return__: `forEach` 내부의 `return`은 의미 없습니다. 제거하세요.
- __인덱싱 일관성__: `answer.at(-1)`을 쓰거나 `answer[answer.length-1]`로 통일해 가독성을 높이세요. 현재 리뷰 상단 예시와 내 코드가 혼재.
- __while 조건 단순화__: `stackLength`를 별도 변수로 관리하기보다, 아래처럼 즉시성 조건으로 단순화 가능.
  ```javascript
  while (k > 0 && answer.length && answer[answer.length - 1] < e) {
    answer.pop();
    k--;
  }
  ```
- __반복문 선택__: `forEach`는 `break/continue`가 어려워 미세 최적화 여지가 줄어듭니다. 일반 `for...of` 또는 인덱스 `for`가 유지보수에 유리.
- __잔여 k 처리__: 후단 `slice(0, number.length - k)`로 남은 `k`를 자연스럽게 잘라내는 처리가 올바릅니다. 이 부분이 누락되면 반례(`"999"`, `k=2`)에서 실패.

# 경계/성능 고려
- __대용량 입력__: 최대 1,000,000자리에서도 배열 push/pop만 수행하므로 성능상 안전. 단, 문자열 분할(`split("")`)과 `join("")` 비용은 불가피.
- __조기 종료 고려__: `k === 0`이면 남은 문자를 그대로 이어붙이는 조기 종료를 넣어도 좋지만, 현재 구현도 사실상 O(n)이라 필수는 아님.

# 제안하는 정리 버전
```javascript
function solution(number, k) {
  const answer = [];
  for (const e of number) {
    while (k > 0 && answer.length && answer[answer.length - 1] < e) {
      answer.pop();
      k--;
    }
    answer.push(e);
  }
  return answer.slice(0, number.length - k).join("");
}
```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250902js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

# 🖱️참고 링크

[MDN- forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

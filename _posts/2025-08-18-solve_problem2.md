---
layout: single
title: "[Programmers] 42586/기능개발/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 자료구조
  - 정렬
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| progresses               | speeds             |
| ------------------------ | ------------------ |
| [93, 30, 55]             | [1, 30, 5]         |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result  |
| ------- |
| [2,1]   |
| [1,3,2] |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[93, 30, 55]
[1, 30, 5]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
[2,1]
```

# 💡 풀이

## ✍️ 풀이과정

reduce 사용법에 익숙하지 않아서 면접 떨어진게 컷는지, 자꾸 쓰게만 됨.
최대한 js 처럼 하려고 for를 지양하려고 노력함(이게 맞는지 모르겠음). 물론 가독성은 조금 떨어진다고 생각함. 근데 이것들을 for문으로 바꿀 수 있으면 된다고 생각함.

다른 사람 풀이 보니까 내가 제일 깔끔허이 좋은듯? ㅋㅋ

## 📖내가 작성한 JS Code

```javascript
function solution(progresses, speeds) {
  return progresses
    .map((v, idx) => Math.ceil((100 - v) / speeds[idx]))
    .reduce(
      (acc, cur) => {
        if (acc[1] < cur) {
          acc[1] = cur;
          acc[0].push(1);
        } else {
          acc[0][acc[0].length - 1]++;
        }
        return acc;
      },
      [[], 0]
    )[0];
}
```

# 🧠 코드 리뷰
* **정확성**: `days = ceil((100 - progress)/speed)`로 각 작업 완료 일수를 구한 뒤, 현재 배치의 기준일(최대 일수)보다 큰 값이 나오면 새 배치 시작, 아니면 직전 배치 카운트 증가 → 요구사항에 부합합니다.
* **복잡도**: `map` O(n) + `reduce` O(n) = O(n). 입력 최대 100이므로 충분히 효율적. 추가 공간은 `map`으로 생성한 일수 배열 O(n).
* **가독성**: 누산기를 `[배치배열, 기준일]` 튜플로 쓰면 의미가 즉시 드러나지 않습니다. 객체를 사용하면 더 읽기 쉽습니다.
* **경계 케이스**: 빈 배열은 `[]`, 이미 완료(100%)도 올바르게 묶입니다. 음수/0 속도는 문제 제약상 없음.

## 개선안 (가독성 향상 + 단일 reduce)
```javascript
function solution(progresses, speeds) {
  const { batches } = progresses.reduce(
    (acc, p, i) => {
      const days = Math.ceil((100 - p) / speeds[i]);
      if (acc.max < days) {
        acc.max = days;
        acc.batches.push(1);
      } else {
        acc.batches[acc.batches.length - 1]++;
      }
      return acc;
    },
    { batches: [], max: 0 }
  );
  return batches;
}
```

## 현재 스타일 유지하며 의미 드러내기(튜플 명시화)
```javascript
function solution(progresses, speeds) {
  const [batches] = progresses
    .map((p, i) => Math.ceil((100 - p) / speeds[i]))
    .reduce(
      (acc, cur) => {
        const [arr, max] = acc;
        if (max < cur) {
          acc[1] = cur;
          arr.push(1);
        } else {
          arr[arr.length - 1]++;
        }
        return acc;
      },
      [[], 0]
    );
  return batches;
}
```

참고: `Math.ceil` 대신 정수 올림을 쓰려면 `const days = Math.floor((100 - p + speeds[i] - 1) / speeds[i]);` 도 가능합니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250818js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)<br>

[MDN- map(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

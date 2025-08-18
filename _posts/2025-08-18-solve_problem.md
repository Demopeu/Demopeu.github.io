---
layout: single
title: "[Programmers] 12906/같은 숫자는 싫어/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| arr             |
| --------------- |
| [1,1,3,3,0,1,1] |
| [4,4,4,3,3]     |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result    |
| --------- |
| [1,3,0,1] |
| [4,3]     |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 배열 arr의 크기 : 1,000,000 이하의 자연수
- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[1,1,3,3,0,1,1]
[4,4,4,3,3]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
[1,3,0,1]
[4,3]
```

# 💡 풀이

## ✍️ 풀이과정

진짜 쉬운 문제인데, filter를 너무 오랜만에 씀. 이런거 땜에 코테 공부 다시함
그리고 고민해서 reduce로도 해봄. reduce 사용법에 익숙해지고 싶어서 그럼. 그런데 효율성에서 전부 시간 초과 뜸.

```javascript
function solution(arr) {
  return arr.reduce(
    (acc, cur) => (acc.at(-1) !== cur ? [...acc, cur] : acc),
    []
  );
}
```

그런데 ai가 더 좋은 reduce 구현을 제시해줬음.

```javascript
// O(n), 메모리 재사용
function solution(arr) {
  return arr.reduce((acc, cur) => {
    if (acc[acc.length - 1] !== cur) acc.push(cur);
    return acc;
  }, []);
}
```

그와중에, 이게 filter 보다 더 빠름;;

그리고, 친구한테 왜 return에 if 쓰면 안되냐 라는 질문을 받았음. return은 expression이 필요한 자리라서 안된다고 했는데,

expression : 값이 될 수 있는 것

- 리터럴: 1, "a", true, [], {}
- 식 결합: a + b, obj.x, fn(), new Date()
- 대입/논리/삼항: x = 3, a && b, cond ? A : B
- 함수/화살표 함수 “표현식”: (function() {}), (x) => x + 1
- JSX: <Div /> (TSX/JSX에서는 이것도 표현식)

statement : 문장

- 선언류: function f(){}, class C {}, var/let/const x = 1;, import/export
- 제어문: if, for, while, switch, try/catch/finally
- 기타: try...catch, throw, return, break, continue, 블록 { ... }

이정도로 분류 가능한듯?

## 📖내가 작성한 JS Code

```javascript
function solution(arr) {
  return arr.filter((num, i) => num !== arr[i + 1]);
}
```

# 🧠 코드 리뷰

- **정확성**: `arr.filter((num, i) => num !== arr[i + 1])`는 연속 중복을 제거하며 각 구간의 "마지막 원소"를 보존합니다. 예시 입력에 대해 기대 결과와 일치합니다.
- **경계 처리**: 마지막 인덱스에서 `arr[i + 1]`는 `undefined`여서 마지막 원소는 항상 포함됩니다. 빈 배열도 문제 없습니다.
- **시간/공간 복잡도**: 필터 1회 순회로 O(n), 추가 공간 O(1)입니다.
- **가독성/의도**: 다음 값과 비교해 "마지막을 남긴다"는 의도가 드러납니다. 첫 값을 남기고 싶다면 이전 값과 비교하는 형태가 더 익숙합니다.

```javascript
// 첫 원소 보존(구간의 첫 값 유지)
const solution = (arr) => arr.filter((v, i) => i === 0 || arr[i - 1] !== v);
```

- **데이터 값 주의**: 배열에 실제로 `undefined`가 포함된 경우에도 연속 구간의 마지막 `undefined`만 남기는 동작으로 일관됩니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250818js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

# 🖱️참고 링크

[MDN- filter(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)<br>
[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

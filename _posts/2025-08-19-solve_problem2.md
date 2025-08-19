---
layout: single
title: "[Programmers] 42587/프로세스/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| priorities           | location             |
| ------------------------ | ------------------ |
| [2, 1, 3, 2]            | 2      |
| [1, 1, 99, 1, 1, 1] | 0 |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result  |
| ------- |
| 1  |
| 5 |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- priorities의 길이는 1 이상 100 이하입니다.
- priorities의 원소는 1 이상 9 이하의 정수입니다.
- priorities의 원소는 우선순위를 나타내며 숫자가 클 수록 우선순위가 높습니다.
- location은 0 이상 (대기 큐에 있는 프로세스 수 - 1) 이하의 값을 가집니다.
- priorities의 가장 앞에 있으면 0, 두 번째에 있으면 1 … 과 같이 표현합니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[2, 1, 3, 2]
2
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
1
```

# 💡 풀이

## ✍️ 풀이과정

그냥 원래 풀던 식으로 풀었음. 머리가 안돌아가나 했는데 다른 사람도 이렇게 풀어서 그냥 그려려니 하고 넘어감. 그런데 최대 큰게 아니라 dict로 해도 됬을듯.

## 📖내가 작성한 JS Code

```javascript
function solution(priorities, location) {
    let answer = 1
    let MAXPRIORITY = Math.max(...priorities)
    while (true){
        const node =  priorities.shift();
        if (node === MAXPRIORITY){
            if (location === 0) return answer
            MAXPRIORITY =  Math.max(...priorities);
            answer++;
        } else {
            priorities.push(node)
        }
        if (location > 0 ){
            location -= 1
        } else{
            location = priorities.length - 1
        }
    }
}
```

# 🧠 코드 리뷰

- **동작 요약**
  - 대기 큐를 배열로 두고 `shift`/`push`로 회전시킵니다. 현재 최대 우선순위(`MAXPRIORITY`)와 비교해 같으면 인쇄하며, 대상 인덱스(`location`)를 함께 회전시켜 추적합니다. 인쇄 시 대상이면 즉시 `answer` 반환.

- **좋은 점**
  - 구현이 직관적이며 문제 정의에 맞게 시뮬레이션합니다.
  - 불필요한 자료구조 없이 간단히 해결합니다.

- **개선 포인트**
  - 시간 복잡도: 인쇄가 일어날 때마다 `Math.max(...priorities)`가 O(n)으로 재계산 → 최악 O(n^2). 우선순위 값 범위가 1..9로 작으므로 빈도 카운트(버킷)를 쓰면 O(1)로 갱신 가능.
  - 입력 배열 `priorities`를 파괴적으로 변경합니다. 호출자가 재사용할 수 있게 하려면 복사본을 사용하세요.
  - 네이밍: `MAXPRIORITY`는 상수 표기처럼 보이지만 값이 변합니다. `currentMax`처럼 소문자 카멜 케이스 권장.
  - 조기 반환은 좋지만, while(true) 보다는 종료 조건을 명시하면 가독성이 올라갑니다.

- **권장 구현(버킷 + 인덱스 큐, O(n))**
  ```javascript
  function solution(priorities, location) {
    const queue = priorities.map((p, i) => ({ p, i }));
    const count = Array(10).fill(0);
    for (const p of priorities) count[p]++;

    let currentMax = 9;
    while (currentMax > 0 && count[currentMax] === 0) currentMax--;

    let printed = 0;
    while (queue.length) {
      const node = queue.shift();
      if (node.p === currentMax) {
        printed++;
        if (node.i === location) return printed;
        if (--count[currentMax] === 0) {
          while (currentMax > 0 && count[currentMax] === 0) currentMax--;
        }
      } else {
        queue.push(node);
      }
    }
    return printed;
  }
  ```

- **복잡도**
  - 현재 코드: 평균 O(n·k) ~ 최악 O(n^2) (반복적인 `max` 재계산).
  - 권장안: O(n) (버킷으로 현재 최대 우선순위 추적).

- **문서 관련(선택)**
  - 하단 링크가 42586으로 연결됩니다. 이 포스트는 42587이므로 링크 교정 권장.
  - front matter 이미지와 본문 첫 이미지가 중복 노출될 수 있습니다. 한쪽만 유지 권장.

  
# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250819js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

# 🖱️참고 링크

[MDN- for(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for)

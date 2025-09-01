---
layout: single
title: "[Programmers] 42885/구명보트/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 그리디알고리즘
  - 투포인터

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42885)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| people        | limit |
| ------------- | ----- |
| [70,50,80,50] | 100   |
| [70,80,50]    | 100   |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 3      |
| 3      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 무인도에 갇힌 사람은 1명 이상 50,000명 이하입니다.
- 각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 항상 사람들의 몸무게 중 최댓값보다 크게 주어지므로 사람들을 구출할 수 없는 경우는 없습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
people: [70,50,80,50]
limit: 100
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
3
```

# 💡 풀이

## ✍️ 풀이과정

단순한 투포인터 문제.

## 📖내가 작성한 JS Code

```javascript
function solution(people, limit) {
  people.sort((a, b) => a - b);
  let l = 0,
    r = people.length - 1,
    answer = 0;

  while (l <= r) {
    if (people[l] + people[r] <= limit) l++;
    r--;
    answer++;
  }
  return answer;
}
```

# 🧠 코드 리뷰

- **장점**

  - 정석 그리디/투포인터 풀이: 오름차순 정렬 후 최솟값(l)과 최댓값(r)을 매칭해 보트 수를 최소화하는 전략이 정확하고 간결함.
  - 시간/공간 효율: 정렬 O(N log N) + 투포인터 O(N), 추가 메모리 O(1)로 최적에 가깝습니다.

- **정확성/엣지 케이스**

  - `while (l <= r)`로 단독 탑승(마지막 1명) 케이스를 자연스럽게 처리함.
  - 제약상 `limit >= max(people)`이므로 구조 불가 케이스는 없음.
  - 동일 체중 다수, 모두 짝이 안 맞는 경우(전부 단독 탑승)도 올바르게 계산됨.

- **가독성/유지보수**

  - 변수 `l`, `r`는 관용적이지만, `left`, `right`로 풀어 쓰면 가독성이 조금 더 높아질 수 있음.
  - `people.sort(...)`가 원본 배열을 변형합니다. 외부에서 `people`을 재사용할 수 있다면, 사본(`const arr = [...people]`)으로 정렬하는 편이 안전합니다.

- **마이너 개선 제안**

  - 고유 의미가 있는 상수/변수 네이밍: `answer` → `boats` 등 결과 의미를 드러내면 좋습니다.
  - 함수 주석에 “정렬로 인한 원본 변형” 여부 명시 권장.
  - 글 메타와 일치성: 현재 글 태그에 `완전탐색`, `dfs`가 포함돼 있으나, 본 문제는 그리디/투포인터입니다. 태그 정정 권장.
  - 하단 링크가 다른 문제(42862)로 연결됩니다. 42885 링크로 수정 권장.

- **대안 구현(원본 불변 + 의미 있는 변수명 예시)**

  ```javascript
  function solution(people, limit) {
    const arr = [...people].sort((a, b) => a - b); // 원본 보존
    let left = 0;
    let right = arr.length - 1;
    let boats = 0;
    while (left <= right) {
      if (arr[left] + arr[right] <= limit) left++;
      right--;
      boats++;
    }
    return boats;
  }
  ```

- **권장 테스트 케이스**
  - 기본: `people=[70,50,80,50], limit=100 → 3`
  - 모두 단독 탑승: `people=[60,60,60], limit=100 → 3`
  - 모두 짝 가능: `people=[40,40,40,40], limit=80 → 2`
  - 큰 값 우세: `people=[90,90,40], limit=100 → 3`
  - 혼합: `people=[40,55,60,85], limit=100 → 3`

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250901js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42885)

# 🖱️참고 링크

[MDN-sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

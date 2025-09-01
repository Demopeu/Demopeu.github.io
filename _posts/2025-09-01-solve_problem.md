---
layout: single
title: "[Programmers] 42862#/체육복/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 완전탐색
  - dfs

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| n   | lost  | reserve |
| --- | ----- | ------- |
| 5   | [2,4] | [1,3,5] |
| 5   | [2,4] | [3]     |
| 3   | [3]   | [1]     |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 5      |
| 4      |
| 2      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
n: 5
lost: [2,4]
reserve: [1,3,5]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
5
```

# 💡 풀이

## ✍️ 풀이과정

처음에 막 틀렸음. sort를 안했음. 이후에 몇개가 또 틀렸길래 확인해보니까 여벌 체육복을 가지고 왔는데 도난당한 학생은 못 빌려준다는 것도 있었음. 옛날에 풀었을 때는 이렇게 제한사항이 많이 없었던 거 같은데 신기하다.

## 📖내가 작성한 JS Code

```javascript
function solution(n, lost, reserve) {
  const newLost = lost
    .filter((v) => !reserve.includes(v))
    .sort((a, b) => a - b);
  let newReserve = reserve
    .filter((v) => !lost.includes(v))
    .sort((a, b) => a - b);

  return newLost.reduce((acc, cur) => {
    for (let i = 0; i < newReserve.length; i++) {
      if (cur - 1 === newReserve[i]) {
        newReserve = newReserve.slice(i + 1);
        acc++;
        break;
      }
      if (cur + 1 === newReserve[i]) {
        acc++;
        newReserve = newReserve.slice(i + 1);
        break;
      }
    }
    return acc;
  }, n - newLost.length);
}
```

# 🧠 코드 리뷰

- **장점**

  - 중복 제거: `lost`와 `reserve`의 교집합을 먼저 필터링하여 문제의 함정을 정확히 처리함.
  - 정렬 후 그리디: 오름차순 정렬 후 앞에서부터 빌려주는 그리디 전략은 직관적이고 정답을 보장함.

- **복잡도/성능**

  - 현재 구현은 `reduce` 내부에서 `newReserve`를 순회하며, `slice`로 배열을 잘라내는 방식으로 상태를 갱신함.
  - `slice`는 매 호출 시 새 배열을 생성하므로 최악의 경우 O(L×R) + 추가 메모리 비용이 발생할 수 있음.
  - `includes` 기반 필터링도 O(N) 탐색이 누적됨. Set을 사용하면 교집합/차집합 연산을 O(1) 평균으로 줄일 수 있음.

- **정확성/엣지 케이스**

  - 경계 학생(1, n)에 대한 처리: 현재 로직은 `cur-1`, `cur+1`만 비교하므로 문제 없음.
  - 동점 후보(좌/우 모두 가능한 경우) 처리: 왼쪽을 우선으로 시도한 뒤 오른쪽을 시도하는 일관된 기준을 가짐.
  - 중복 대여 방지: `slice(i+1)`로 사용된 이전/현재 인덱스 이전의 여벌은 재사용되지 않도록 보장됨.

- **가독성/유지보수**

  - 변수 네이밍은 명확하나, `newReserve = newReserve.slice(i + 1)`는 의도가 바로 눈에 들어오지 않을 수 있음(“포인터 전진” 의미를 코드화한 것임을 주석으로 보완 권장).
  - 불변성 유지 측면에서는 좋지만, 실질적으로 매 루프 새 배열을 만드는 점은 성능-가독성 트레이드오프가 존재.

- **개선 제안(두 포인터 + Set)**

  - 교집합 제거는 Set으로, 매칭은 두 포인터로 처리하면 시간/공간 효율을 개선할 수 있음.

  ```javascript
  function solution(n, lost, reserve) {
    const lostSet = new Set(lost);
    const reserveSet = new Set(reserve);
    // 교집합 제거
    for (const s of reserve) {
      if (lostSet.has(s)) {
        lostSet.delete(s);
        reserveSet.delete(s);
      }
    }
    const need = [...lostSet].sort((a, b) => a - b);
    const extra = [...reserveSet].sort((a, b) => a - b);
    let i = 0,
      j = 0,
      borrowed = 0;
    while (i < need.length && j < extra.length) {
      if (Math.abs(need[i] - extra[j]) === 1) {
        borrowed++;
        i++;
        j++;
      } else if (extra[j] < need[i] - 1) {
        j++;
      } else {
        i++;
      }
    }
    return n - need.length + borrowed;
  }
  ```

- **테스트 제안**

  - 기본: `n=5, lost=[2,4], reserve=[1,3,5] → 5`
  - 겹침: `n=5, lost=[2,4], reserve=[2,3] → 4`
  - 경계: `n=3, lost=[1], reserve=[2] → 3`, `n=3, lost=[3], reserve=[2] → 3`
  - 모두 겹침: `n=4, lost=[2,3], reserve=[2,3] → 4`

- **마이너 개선**
  - `const`로 선언한 `newReserve`는 재할당이 반복되므로 `let` 사용은 적절함. 다만 “소모형 큐”의 의도를 주석으로 명시.
  - 필터 단계의 `includes`는 입력 크기가 커질수록 비용이 커지므로 Set 전환을 고려.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250901js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42862#)

# 🖱️참고 링크

[MDN- 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

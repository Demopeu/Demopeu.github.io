---
layout: single
title: "[Programmers] 42840/모의고사/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42840)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| answers          |
| --------------- |
| [1,2,3,4,5] |
| [1,3,2,4,2] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result          |
| --------------- |
| [1] |
| [1, 2,3] |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[1, 2, 3, 4, 5]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
[1]
```

# 💡 풀이

## ✍️ 풀이과정

패턴을 저장하고 대입하면 되는 문제.

## 📖내가 작성한 JS Code

```javascript
function solution(answers) {
    const supoza1 = [1,2,3,4,5];
    const supoza2 = [2,1,2,3,2,4,2,5];
    const supoza3 = [3,3,1,1,2,2,4,4,5,5,];
    const answer =  answers.reduce((acc,cur,idx)=>[cur===supoza1[idx%5]? acc[0]+1:acc[0],cur===supoza2[idx%8]? acc[1]+1:acc[1],cur===supoza3[idx%10]? acc[2]+1:acc[2]],[0,0,0]);
    const MAXNUMBER = Math.max(...answer)
    return answer.reduce((acc,cur,idx)=>{
        if (cur === MAXNUMBER) acc.push(idx+1)
            return acc 
    },[])
}
```

# 🧠 코드 리뷰

- __네이밍__: `supoza1/2/3`는 의미 파악이 어렵습니다. `patterns`, `p1/p2/p3`처럼 의도를 드러내는 이름이 좋습니다.
- __불필요한 배열 생성__: `answers.reduce((acc, cur, idx) => [ ... ])`는 반복마다 새 배열을 만듭니다. 카운터 변수 3개로 누적하면 메모리 할당을 줄이고 가독성이 좋아집니다.
- __트레일링 콤마__: `supoza3`에 불필요한 끝 콤마가 있습니다(`[5,5,]`). 동작엔 영향 없지만 제거 권장.
- __가독성__: 이중 `reduce` 대신 한 번의 순회로 점수를 집계하고, 최댓값과 동점자 추출을 분리하면 읽기 쉽습니다.
- __복잡도__: 시간 O(n), 공간 O(1). 현재도 동일하나 상수 비용을 줄일 수 있습니다.
- __엣지 케이스__: `answers`가 빈 배열이면 모든 수포자 점수가 0이 되어 `[1,2,3]` 반환이 직관적입니다(문제 의도와도 부합).

### ✅ 개선된 JS 코드 (단일 순회, 비생성/비변이)

```javascript
function solution(answers) {
  const p1 = [1, 2, 3, 4, 5];
  const p2 = [2, 1, 2, 3, 2, 4, 2, 5];
  const p3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];

  let s1 = 0, s2 = 0, s3 = 0;
  for (let i = 0; i < answers.length; i++) {
    const a = answers[i];
    if (a === p1[i % p1.length]) s1++;
    if (a === p2[i % p2.length]) s2++;
    if (a === p3[i % p3.length]) s3++;
  }

  const scores = [s1, s2, s3];
  const max = Math.max(...scores);
  const result = [];
  for (let i = 0; i < scores.length; i++) {
    if (scores[i] === max) result.push(i + 1);
  }
  return result; // 오름차순 보장 (1→3 순회)
}
```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250822js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42840)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
[MDN- for(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for)

---
layout: single
title: "[Programmers] 43105/정수삼각형/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 동적계획법
  - 다이나믹프로그래밍

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/43105)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| triangle                                    |
| ----------------------------------------- |
| [[7], [3,8], [8,1,0], [2,7,4,4], [4,5,2,6,5]] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 30     |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 삼각형의 높이는 1 이상 500 이하입니다.
- 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```javascript
triangle: [
  [7],
  [3,8],
  [8,1,0],
  [2,7,4,4],
  [4,5,2,6,5],
];
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```javascript
30;
```

# 💡 풀이

## ✍️ 풀이과정

다이나믹프로그래밍의 팁은 항상 작은거부터 직접 해보는게 제일 중요한듯. 이것도 [7],[3,8]에서 [10]이 되는게 맞는지 확인하고 [10],[8,1,0]에서 [18,11]이 되는지 확인하고 [18,11],[2,7,4,4]에서 [20,15,15]이 되는지 확인하고 [20,15,15],[4,5,2,6,5]에서 [24,21,20,21]이 되는지 확인하고 [24,21,20,21],[30,27,25,26,26]이 되는지 확인하고 [30,27,25,26,26]이 최대값이 되는지 확인.

## 📖내가 작성한 JS Code

```javascript
function solution(triangle) {
    const reverseTriangle =  triangle.reverse();
    reverseTriangle.slice(1).forEach((ele,idx)=>{
        idx++;
        for (let i = 0;i<ele.length;i++) reverseTriangle[idx][i] += Math.max(reverseTriangle[idx-1][i],reverseTriangle[idx-1][i+1]);
    })
    return  reverseTriangle[reverseTriangle.length-1][0];
}
```

# 🧠 코드 리뷰

- 장점
  - 하향식이 아닌 상향식(bottom-up) DP로 접근해 각 행을 누적하며 최대 합을 계산하는 방식이 정석적입니다.
  - 시간 복잡도 O(N^2), 추가 공간 O(1)에 가까운 형태로 효율적입니다. 여기서 N은 삼각형의 높이입니다.

- 개선이 필요한 부분/권장 리팩터링
  1) 입력 변형(side-effect)
     - `triangle.reverse()`는 원본 입력 배열의 순서를 바꿉니다. 더 나아가 현재 구현은 내부 원소도 갱신하므로(예: `reverseTriangle[idx][i] += ...`) 얕은 복사만으로는 원본의 내부 배열까지 변형될 수 있습니다.
     - 문제 환경에서는 부작용이 치명적이지 않을 수 있으나, 함수의 참조 투명성과 재사용성을 위해 입력을 변형하지 않는 구현이 바람직합니다.
     - 대안: 1차원 DP로 마지막 행만 복사해 갱신하면 원본을 전혀 바꾸지 않으면서도 공간을 최소화할 수 있습니다. 예시:

       ```javascript
       function solution(triangle) {
         const n = triangle.length;
         const dp = triangle[n - 1].slice(); // 마지막 행 복사
         for (let r = n - 2; r >= 0; r--) {
           for (let c = 0; c <= r; c++) {
             dp[c] = triangle[r][c] + Math.max(dp[c], dp[c + 1]);
           }
         }
         return dp[0];
       }
       ```

  2) 인덱스 처리 가독성
     - `reverseTriangle.slice(1).forEach((ele, idx) => { idx++; ... })` 는 콜백 인덱스를 다시 1 증가시키는 패턴이라 읽는 이가 혼동하기 쉽습니다.
     - 명시적 for 루프(`for (let r = 1; r < reverseTriangle.length; r++)`) 또는 `for...of` + 별도 행 인덱스를 권장합니다.

  3) 변수명/일관성
     - `ele`, `idx` 대신 `row`, `r`, `c`처럼 의미가 드러나는 이름을 사용하면 좋습니다.
     - 공백과 세미콜론 사용을 일관되게 유지하세요. 현재 `return  reverseTriangle...` 처럼 이중 공백이 보입니다.

  4) 경계 조건 명시
     - 삼각형의 성질상 `reverseTriangle[idx-1][i+1]`는 항상 존재하지만, 이 불변식을 주석으로 남겨두면 향후 유지보수 시 안전합니다.

- 정확성
  - 하향식에서 상향식으로의 변환이 올바르며, 각 셀에 대해 바로 아래 두 값 중 큰 값을 더하는 점화식을 정확히 구현했습니다.

- 복잡도
  - 시간: 모든 원소를 한 번씩 방문하므로 O(N^2)
  - 공간: 현재 구현은 입력을 직접 갱신하므로 추가 공간 O(1). 위의 1차원 DP(불변성 유지)도 O(N) 추가 공간으로 충분히 작습니다.

- 테스트/안전성 제안
  - 최소 높이(1), 최대 높이(500), 모든 값이 동일한 경우, 한쪽 경로가 압도적으로 큰 경우 등 다양한 케이스를 빠르게 점검해보는 것을 권장합니다.

- 요약
  - 알고리즘 선택과 점화식은 적절합니다. 다만 입력 변형을 피하고 인덱싱/변수명 가독성을 개선하면 더 견고하고 재사용 가능한 풀이가 됩니다. 실무 관점에서는 1차원 DP로 불변성을 유지하는 버전을 추천합니다.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250905js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/43105)

# 🖱️참고 링크

[MDN- forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

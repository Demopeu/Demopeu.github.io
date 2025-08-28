---
layout: single
title: "[Programmers] 42839/소수 찾기/javascript"
categories: 문제풀이
tags:
  - javascript
  - 프로그래머스
  - 알고리즘
  - 완전탐색
  - 정수론

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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| numbers |
| ------- |
| "17"    |
| "011"   |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 3      |
| 2      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
numbers: "17"
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
3
```

# 💡 풀이

## ✍️ 풀이과정

js에는 유용한 라이브러리가 참 없다. 그래서 그냥 dfs로 전부 만들었음. 시간 초과가 안날꺼라 전부 만들었고, 백트래킹도 쓰지 않았음.

## 📖내가 작성한 JS Code

```javascript
function solution(numbers) {
    const eachNumbers = numbers.split('').sort();
    const visited = Array(eachNumbers.length).fill(false);
    const answer = new Set();
    
    const getPertations = (cur) =>{
        if (cur.length) answer.add(Number(cur.join('')));
        for (let i = 0; i<eachNumbers.length; i++){
            if (visited[i]) continue;
            visited[i] = true;
            cur.push(eachNumbers[i]);
            getPertations(cur);
            cur.pop();
            visited[i] = false;
            
        }
    }
    
    const isPrime = (num) =>{
        if (num < 2) return false;
        for (let i = 2; i<= Math.sqrt(num);i++){
            if (num % i === 0) return false;
        }
        return true;
    }
    
    getPertations([])
    
    return [...answer].reduce((acc,cur)=>{
        if (isPrime(cur)) acc++;
        return acc;
    },0);
}
```

# 🧠 코드 리뷰
 - **장점**
   - **완전탐색/백트래킹 적절**: `visited`로 자릿수 중복 사용 방지, 모든 길이 조합 생성.
   - **중복 제거**: 최종 숫자 집합을 `Set`으로 관리해 결과 중복을 방지.
   - **소수 판별의 기본 최적화**: `Math.sqrt(num)`까지만 검사.
 
 - **개선점**
   - **오탈자**: `getPertations` → `getPermutations`로 변경 권장(가독성).
   - **중복 가지치기 미적용**: 입력이 정렬되어 있어도 동일 깊이에서 같은 숫자를 스킵하지 않아 불필요한 분기가 발생. 다음 패턴 추천:
     - 정렬 유지 + `if (i > 0 && eachNumbers[i] === eachNumbers[i-1] && !visited[i-1]) continue;`
   - **스타일 일관성**: 세미콜론 누락(`getPertations([])`) 등 코드 스타일 통일 권장.
   - **네이밍 개선**: `eachNumbers` → `digits`, `answer` → `candidates` 등 의미가 더 분명한 이름 추천.
   - **소수 판별 미세 최적화(선택)**: `2` 예외 처리, 짝수 early return, 3부터 홀수만 검사.
 
 - **리팩터링 제안 예시(아이디어)**
   - 백트래킹:
     - 동일 깊이에서 이미 선택하지 않은 동일 숫자 스킵: `if (i > 0 && digits[i] === digits[i-1] && !visited[i-1]) continue;`
   - 소수 판별:
     - `if (num === 2) return true; if (num % 2 === 0) return false; for (let i = 3; i <= Math.sqrt(num); i += 2) { ... }`
   - 카운트:
     - `reduce` 대신 단순 루프가 가독성 좋음.

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250828js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

# 🖱️참고 링크

[MDN- 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

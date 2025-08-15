---
layout: single
title: "[Programmers] 1845/폰켓몬/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/1845)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| nums          |
| ------------- |
| [3,1,2,3]     |
| [3,3,3,2,2,4] |
| [3,3,3,2,2,2] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| 2      |
| 3      |
| 2      |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- nums는 폰켓몬의 종류 번호가 담긴 1차원 배열입니다.
- nums의 길이(N)는 1 이상 10,000 이하의 자연수이며, 항상 짝수로 주어집니다.
- 폰켓몬의 종류 번호는 1 이상 200,000 이하의 자연수로 나타냅니다.
- 가장 많은 종류의 폰켓몬을 선택하는 방법이 여러 가지인 경우에도, 선택할 수 있는 폰켓몬 종류 개수의 최댓값 하나만 return 하면 됩니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
[3,1,2,3]
[3,3,3,2,2,4]
[3,3,3,2,2,2]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
2
3
2
```

# 💡 풀이

## ✍️ 풀이과정

생각을 좀 정리하고 왔는데, 이번 현대오토에버와 토스 코딩테스트를 경험하고 어려운 문제가 아니라 기초부터 다시 잡아야겠다는 생각을 하였다.
그래서 프로그래머스 고득점 키트 하루에 2개씩 푸는걸로 대체할듯. 어려우면 1개씩으로?
해시 문제였지만, 간단하게 구현으로 가능.

여기서 중요한 것은 js에서는 Set의 경우 new Set(arr)을 사용하여 생성. 그리고 사이즈는 set.size를 사용한다.
그리고 속도는 내부적으로 해시 구조라 평균 접근/삽입은 O(1).(최악은 O(n)인데 이거는 해시 충돌이니까 그려려니 하자)
has(),size,add(),delete()만 알아놔도 될듯.

## 📖내가 작성한 JS Code

```javascript
function solution(nums) {
  return new Set(nums).size <= nums.length / 2
    ? new Set(nums).size
    : nums.length / 2;
}
```

# 🧠 코드 리뷰

- 장점: Set으로 중복 제거해 의도를 명확히 드러냈고, 코드가 매우 간결합니다. 시간복잡도 O(N), 공간복잡도 O(K)로 적절합니다.
- 개선점: new Set(nums)를 두 번 생성하고 있어 불필요한 객체 생성 비용이 있습니다. 한 번만 생성해 캐싱하는 편이 좋습니다.
- 가독성: Math.min을 사용하면 “고유 종류 수와 선택 가능 수 중 작은 값”이라는 의도가 더 즉각적으로 읽힙니다.

추천 코드:

```javascript
function solution(nums) {
  const distinctCount = new Set(nums).size; // 중복 제거 1회만 수행
  const pick = nums.length / 2; // 문제 조건상 항상 정수
  return Math.min(distinctCount, pick);
}
```

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250815js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/1845)

# 🖱️참고 링크

[Set - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)

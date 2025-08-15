---
layout: single
title: "[Programmers] 42577/전화번호 목록/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/42577)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| phone_book                         |
| ---------------------------------- |
| ["119", "97674223", "1195524421"]  |
| ["123", "456", "789"]              |
| ["12", "123", "1235", "567", "88"] |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result |
| ------ |
| false  |
| true   |
| false  |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
  - 각 전화번호의 길이는 1 이상 20 이하입니다.
  - 같은 전화번호가 중복해서 들어있지 않습니다.

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
["119", "97674223", "1195524421"]
["123", "456", "789"]
["12", "123", "1235", "567", "88"]
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
false
true
false
```

# 💡 풀이

## ✍️ 풀이과정

그냥 한바퀴 돌면 되는 이야기. 그런데 forEach의 return은 콜백 함수 내부에서만 동작하기 때문에 사용이 불가능함. 그래서 비슷한 some()과 every()을 사용.

```javascript
function solution(phone_book) {
  return !phone_book
    .sort()
    .some((str, i) =>
      phone_book.some((other, j) => i !== j && other.startsWith(str))
    );
}
```

하지만, 효율성 테스트 3,4에서 시간 초과가 났다. O(n\*\*2)이라서 그런듯.

그래서, js에서는 str을 sort()로 정렬할 수 있으니, 정렬 후에 바로 뒤에 것만 비교해서 통과시킴.
문제 자체에 시간복잡도와 공간복잡도 제한이 적혀있지 않으니, 이렇게 해봐야 아는 건가? 했음.

이 문제에서 알아야할 것은,

1. some(): 조건을 만족하는 요소가 하나라도 있으면 true 반환 후 즉시 종료
2. every(): 조건을 만족하지 않는 요소가 하나라도 있으면 false 반환 후 즉시 종료
3. forEach(): break, continue 불가능. return은 현재 콜백 함수만 종료. 즉, for문 전체를 종료하지 않음.
4. startsWith(): 문자열이 다른 문자열로 시작하는지 확인

여기서 startsWith의 동작 원리를 간단히 보면,

```javascript
function startsWith(str, prefix) {
  if (prefix.length > str.length) return false;
  for (let i = 0; i < prefix.length; i++) {
    if (str[i] !== prefix[i]) return false;
  }
  return true;
}
```

대충 이런데, 문자열 하나하나 비교해서 안좋아 보일 수 있음. 그래도 내부적으로 최적화를 해서 slice보다는 좋음. 그리고 길이 체크가 자동으로 들어가기 때문에 더 편함.

그리고 엣지 케이스에 대한 생각인데, 나는 가장 큰 입력에서만 생각함. 시간적 여유가 있으면 항상 다른 엣지 케이스를 생각하는 시간도 필요할듯.

1. 최소/최대
2. 공통 접두어가 긴 경우
3. 끝에 한글자만 다른 경우

완전히 다른 번호랑, 길이 같은건 그냥 테스트케이스에도 있으니 생략해도 될듯?

## 📖내가 작성한 JS Code

```javascript
function solution(phone_book) {
  return !phone_book
    .sort()
    .some(
      (num, i) => i < phone_book.length - 1 && phone_book[i + 1].startsWith(num)
    );
}
```

# 🧠 코드 리뷰

- 장점: O(N log N) 정렬 후 인접 원소만 비교하는 전략으로 O(N^2) 중첩 탐색을 제거해 효율성 이슈를 해결했습니다. startsWith로 접두 관계를 정확히 판별합니다.
- 개선점:
  - 입력 배열을 .sort()로 직접 변형합니다. 원본 보존이 필요할 수 있으므로 얕은 복사([...phone_book]) 후 정렬을 권장합니다.
  - some 안에서 i < length-1 조건을 매번 평가하는 것보다, 명시적 for 루프가 더 읽기 쉽고 약간 더 빠릅니다(콜백/클로저 오버헤드 제거).
  - 이중 부정(!some(...))은 의도가 한 번 더 해석되어야 합니다. “접두어 발견 시 false, 아니면 true” 로드맵을 반환 분기에서 직접 표현하는 편이 명료합니다.
- 복잡도: 시간 O(N log N) (정렬 지배), 공간 O(1) in-place 정렬 기준(엔진 구현/정렬 알고리즘에 따라 O(N) 보조 메모리 가능). 얕은 복사 사용 시 O(N).
- 엣지 케이스: 길이 0/1 배열은 항상 true. 동일 문자열 중복 입력은 제한상 없지만, 있으면 인접 정렬 비교로 즉시 false 처리됩니다.

추천 코드(원본 불변 + 명료성):

```javascript
function solution(phone_book) {
  const sorted = [...phone_book].sort(); // 원본 보존
  for (let i = 0; i < sorted.length - 1; i++) {
    if (sorted[i + 1].startsWith(sorted[i])) return false; // 접두 관계 발견
  }
  return true; // 접두 관계 없음
}
```

참고:

- 문자열 정렬은 기본적으로 사전식 정렬이므로 접두 검출 로직과 호환됩니다.
- 입력 규모(최대 1e6)에서 메모리 여유가 부족하다면 원본 변형 정렬도 현실적인 선택입니다(문제 요구사항에 따라 결정).

# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250815js2.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/42577)

# 🖱️참고 링크

[MDN- forEach(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

[MDN- startsWith(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)

[MDN- sort(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

[MDN- some(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/some)

[MDN- every(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

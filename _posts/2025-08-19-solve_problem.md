---
layout: single
title: "[Programmers] 12909/올바른 괄호/javascript"
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

[문제 설명](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

<!--more-->

<strong style="font-size: 1.5em">📥 입력</strong>

| s             |
| --------------- |
| "()()" |
| "(()())"     |

<strong style="font-size: 1.5em"> 📤 출력</strong>

| result    |
| --------- |
| true |
| true     |

<strong style="font-size: 1.5em">📏 제한 사항</strong>

- 배열 arr의 크기 : 1,000,000 이하의 자연수
- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

<strong style="font-size: 1.5em">📥 예제 입력</strong>

```
"()()"
"(()())"
```

<strong style="font-size: 1.5em"> 📤 예제 출력</strong>

```
true
true
```

# 💡 풀이

## ✍️ 풀이과정

딱히 어려운 점은 없었는데 String을 어떻게 하나하나 처리할까 하다가 그냥 언팩해봄. 그런데 됨. 사실 이거 보다는 바로 종료하는게 더 효율적이지만, reduce로 함.

## 📖내가 작성한 JS Code

```javascript
function solution(s){
    return ([...s].reduce((acc,cur)=>{
        if (acc === -1){
            return acc
        } else if (cur === "("){
            acc+=1
        } else if(acc>0) {
            acc-=1
        } else{
            return -1
        }
        return acc
        },0)) === 0 ? true : false;
}
```

# 🧠 코드 리뷰



# 💻결과

<strong style="font-size: 1.2em">⚡ Javascript</strong>

![js_result](https://Demopeu.github.io/images/result/20250819js.png)

[프로그래머스 문제 보러가기](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

# 🖱️참고 링크

[MDN- reduce(JavaScript)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

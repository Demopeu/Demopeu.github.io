---
layout: single
title: "[javascript] node.js에서 console.log의 동작 흐름"
categories: 문제풀이
tags:
  - javascript
  - node.js
  - console.log

image:
  path: https://Demopeu.github.io/images/logo/JAVASCRIPT.png
  alt: "JAVASCRIPT"
  thumbnail: true
toc: true
author_profile: false
sidebar:
  nav: "counts"
use_math: true
---

![JAVASCRIPT](https://Demopeu.github.io/images/logo/JAVASCRIPT.png)

## ✅ console.log

이번 코딩테스트를 준비하면서 console.log의 동작 원리에 대하여 공부하게 되었고, 정리해보려고 한다.

여기서는 V8 엔진으로 설명하려고 한다.

## ✅ console.log 동작 흐름

> JavaScript 코드 → V8 Engine → Node.js C++ 바인딩 → libuv → 운영체제 시스템 콜

### 📌 V8엔진 레벨에서의 처리

console.log는 js 함수이지만, 내부는 C++로 동작한다. js 자체는 운영체제에 직접 접근하지 못하기 때문이다.

**따라서, v8엔진은 js코드를 실행하면서 console.log를 만나면 node.js가 미리 바인딩해둔 네이티브 함수임을 인식하고, 그 함수를 호출한다.**

말이 어렵다. 좀 쉽게 설명하면, node.js는 시작할때, console.log 함수 안의 c++ 코드가 동작할 수 있도록 미리 준비해둔다.
그 이후, v8엔진은 js코드를 실행하면서 console.log를 만나면 node.js가 준비해둔 c++코드를 대신 실행한다.

```javascript
function log(...args) {
  process.stdout.write(util.format(...args) + "\n");
}
```

v8엔진은 node.js가 만들어놓은 util.format을 사용하여 인자들을 순서대로 확인하고 문자열로 변환한다.

여기서! 복잡한 객체가 올 경우, node.js가 만들어놓은 util.inspect를 사용하여 문자열로 처리한다. 예를 들어 깊은 배열, 함수, 순환 참조 등이 여기에 속한다.

이후 v8엔진은 process.stdout.write(str)를 실행한다.

### 📌 C++ 레벨에서의 처리

하지만, process.stdout.write(str) 내부는 c++ 코드로 돌아가기 때문에, 이 시점부터 node.js 내부의 c++ 모듈로 실행을 위임한다.

node.js 내부의 c++ 모듈은 libuv 라이브러리를 사용하여 운영체제 시스템 콜을 한다.

이러한 복잡한 과정을 거치게 되면, 우리는 드디어 터미널에서 문자를 볼 수 있다.

> libuv 라이브러리란?
>
> Node.js가 비동기 I/O(입출력)를 처리할 수 있도록 도와주는 크로스 플랫폼(Cross-platform) 라이브러리

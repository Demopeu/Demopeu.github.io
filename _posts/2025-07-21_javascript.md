---
layout: single
title: "[javascript] 참조자료형"
categories: 문제풀이
tags:
  - javascript
  - 자료구조
  - 참조자료형
  - Object

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

## ✅ JavaScript 참조자료형의 중요성

 이따금, React를 사용하면서 상태 업데이트가 안되어 디버깅에 시간을 쏟았던 경험이 있을 것이다. 참조주소가 변경되지 않아 상태가 업데이트되지 않았던 것인데, 이건 JavaScript의 참조자료형의 특성을 이해하지 못함에서 비롯된 것이다.

 어떤 사람들의 경우, useMemo, useCallback의 동작원리를 찾아보다가, 디바운스나 캐싱할 때도 이런 경험들이 있을 것이다. 

 나는 알고리즘 풀면서 메서드가 기억이 안나 이번 기회에 정리해보려고 한다.

## ✅ JavaScript의 참조자료형

참조자료형이란 **복잡한 데이터**를 저장할 수 있는 타입. 변수에는 실제 값이 아닌 메모리 주소가 저장됨.

### ✅ 참조자료형의 특징

#### 📌 메모리 관점

  - heap에 저장, 변수에는 참조 주소가 저장됨.
  - 객체 수명에 따라 가비지 컬렉터가 자동으로 관리.

#### 📌 복사 연산

  - 얕은 복사가 기본. 깊은 복사도 가능.

## ✅ 참조자료형의 종류

### 📌 1. Object

 - 키-값 쌍으로 구성된 가장 기본적인 참조형.
 - 키는 문자열, Symbol로 구성.
 - 순서는 명세상 보장되지 않음.

 ```javascript
 const obj = {a: 1, b: 2};
 ```
 #### 🧰 메서드

 - Object.keys() : 키 배열 반환
 - Object.values() : 값 배열 반환
 - Object.entries() : 키-값 쌍 배열 반환
 - Object.hasOwn(key) : 키 존재 여부 확인

 ```javascript
 console.log(Object.keys(obj)); // ['a', 'b']
 console.log(Object.values(obj)); // [1, 2]
 console.log(Object.entries(obj)); // [['a', 1], ['b', 2]]
 console.log(Object.hasOwn(obj, 'a')); // true
 ```

---

### 📌 Array

  - 순서가 있는 데이터를 저장하는 참조형.
  - 인덱스로 접근 가능.

 ```javascript
 const arr = [1, 2, 3];
 ```

 #### 🧰 순회 & 변형 메서드

- Array.forEach(callback) : 배열 순회
- Array.map(callback) : 새 배열 생성(각 요소 변경 시 사용)
- Array.filter(callback) : 새 배열 생성(조건에 맞는 요소만 추출 시 사용)
- Array.reduce(callback) : 누적값으로 하나의 값 반환
```javascript
const arr = [1, 2, 3];
arr.forEach((item) => console.log(item)); // [1, 2, 3]
arr.map((item) => item * 2); // [2, 4, 6]
arr.filter((item) => item % 2 === 0); // [2]
arr.reduce((acc, cur) => acc + cur, 0); // 6
```
- Array.find(callback) : 조건에 맞는 첫 요소 반환
- Array.findIndex(callback) : 조건에 맞는 첫 요소 인덱스 반환
 ```javascript
 const arr = [1, 2, 3];
 arr.find((item) => item % 2 === 0); // 2
 arr.findIndex((item) => item % 2 === 0); // 1
 ```
- Array.some(callback) : 조건에 맞는 요소가 있는지 확인
- Array.every(callback) : 모든 요소가 조건에 맞는지 확인
 ```javascript
 const arr = [1, 2, 3];
 arr.some((item) => item % 2 === 0); // true
 arr.every((item) => item % 2 === 0); // false
 ```

#### 🧰 추가,삭제,정렬 메서드

- Array.push(item) : 배열 끝에 요소 추가
- Array.pop() : 배열 끝 요소 삭제
- Array.unshift(item) : 배열 앞에 요소 추가
- Array.shift() : 배열 앞 요소 삭제
- Array.sort(callback) : 배열 정렬
- Array.reverse() : 배열 역순
- Array.splice(start, deleteCount, item) : 배열의 일부를 추가, 삭제
 ```javascript
 const arr = [1, 2, 3];
 arr.push(4); // [1, 2, 3, 4]
 arr.pop(); // [1, 2, 3]
 arr.unshift(0); // [0, 1, 2, 3]
 arr.shift(); // [1, 2, 3]
 arr.sort((a, b) => a - b); // [1, 2, 3]
 arr.reverse(); // [3, 2, 1]
 arr.splice(1, 1); // [1, 3]
 ```
#### 🧰 생성 & 복사 메서드

- Array.from(iterable) : 이터러블을 배열로 변환
- Array.of(...items) : 배열 생성
- Array.slice(start, end) : 배열의 일부를 복사
- Array.join(separator) : 배열을 문자열로 변환
- Array.fill(value, start, end) : 배열의 일부를 특정 값으로 채움
- Array.flat(depth) : 배열의 평탄화

 ```javascript
 const arr = [1, 2, 3];
 const arr2 = Array.from(arr); // [1, 2, 3]
 const arr3 = Array.of(1, 2, 3); // [1, 2, 3]
 const arr4 = arr.slice(1, 2); // [2]
 const arr5 = arr.join(","); // "1,2,3"
 const arr6 = arr.fill(0); // [0, 0, 0]
 const arr7 = arr.flat(); // [1, 2, 3]
 ```

#### 🧰  기타 메서드

- Array.indexOf(searchElement, fromIndex) : 첫 번째 일치 요소 인덱스
- Array.lastIndexOf(searchElement, fromIndex) : 마지막 일치 요소 인덱스
- Array.includes(searchElement, fromIndex) : 요소 포함 여부 확인

 ```javascript
 const arr = [1, 2, 3];
 const arr2 = arr.indexOf(2); // 1
 const arr3 = arr.lastIndexOf(2); // 1
 const arr4 = arr.includes(2); // true
 ```

---

### 📌 Map

- Map은 키-값 쌍으로 구성된 객체.
- 키는 모든 타입 가능.
- 순서 보장됨.

 ```javascript
 const map = new Map();
 map.set("key1", "value1");
 map.set("key2", "value2");
 ```

#### 🧰 메서드

- Map.set(key, value) : 키-값 쌍 추가
- Map.get(key) : 키에 해당하는 값 반환
- Map.has(key) : 키 존재 여부 확인
- Map.delete(key) : 키-값 쌍 삭제
- Map.clear() : 모든 키-값 쌍 삭제
- Map.size : 키-값 쌍 개수

 ```javascript
 const map = new Map();
 map.set("key1", "value1");
 map.set("key2", "value2");
 const map2 = map.get("key1"); // "value1"
 const map3 = map.has("key1"); // true
 const map4 = map.delete("key1"); // true
 const map5 = map.clear(); // undefined
 const map6 = map.size; // 0
 ```

---

#### 📌 Set

- Set은 중복되지 않는 값의 집합.
- 순서 보장됨.

 ```javascript
const mySet = new Set();

mySet.add(1);
mySet.add(2);
mySet.add(2);  // 중복 → 무시됨
mySet.add('hello');

console.log(mySet);       // Set(3) { 1, 2, 'hello' }
console.log(mySet.size);  // 3
 ```

#### 🧰  메서드

- Set.add(value) : 값 추가
- Set.delete(value) : 값 삭제
- Set.has(value) : 값 존재 여부 확인
- Set.clear() : 모든 값 삭제
- Set.size : 값 개수

 ```javascript
const mySet = new Set();
mySet.add(1);
mySet.add(2);
mySet.add(2);  // 중복 → 무시됨
mySet.add('hello');

console.log(mySet);       // Set(3) { 1, 2, 'hello' }
console.log(mySet.size);  // 3
 ```

---

### 📌 Function

- 함수는 일급 객체.
- 함수는 객체이므로 프로퍼티와 메서드를 가질 수 있음.(name, length(선언된 매개변수 개수))

 ```javascript
function foo() {
    console.log("foo");
}
foo.bar = 123;
console.log(foo.bar); // 123
console.log(foo.name); // foo
console.log(foo.length); // 0
 ```

---

### 📌 WeakMap & WeakSet

- 키 또는 값이 가비지 컬렉션 대상되면 내부에서도 자동 제거.
- 순회, 크기 확인 불가능
- WeakMap : 키가 객체일 때 사용
- WeakSet : 값이 객체일 때 사용
- DOM 캐싱이나 이벤트 연결 시 사용

---

### 📌 Date, RegExp

- Date : 시간을 다루는 객체
- RegExp : 정규표현식을 다루는 객체


---
title: "[JavaScript] Deep Copy vs Shallow Copy"
excerpt: ""

categories:
  - Web
tags:
  - [Web, JavaScript]

toc: true
toc_sticky: true

date: 2025-04-25
last_modified_at: 2025-04-25
---

## 얉은 복사(Shallow Copy)
얉은 복사란, 객체의 최상위 속성들만 복사하는 방식으로 중첩된 객체 속성은 원본 객체의 속성과 동일한 참조(레퍼런스)를 공유함.

즉, 복사본이나 원본을 변경하면 중첩된 객체의 속성은 서로 영향을 주게 됨
   
```js
const original = {
  name: '홍길동',
  info: {
    age: 30
  }
}
console.log('original: ', original)

const copy = {...original}
copy.name = '김철수'
copy.info.age = 50
console.log('copy: ', copy)
```
   
<h3>✅ 결과</h3>
```
original : {name: '홍길동', info: {age: 50}}
copy : {name: '김철수', info: {age: 50}}
```
   
### 얉은 복사 만드는 방법
1. 스프레드 연산자 (...) 
2. `Object.assign()` 
3. 배열의 `slice()`, `concat()`, `Array.from()`
   

## 깊은 복사(Deep Copy)
깊은 복사란 객체의 모든 속성과 중첩된 객체들을 완전히 새로운 메모리에 복사하는 방식
   
```js
const original = {
  name: '홍길동',
  info: {
    age: 30
  }
}
console.log('original: ', original)

const copy = JSON.parse(JSON.stringfy(original))
copy.name = '김철수'
copy.info.age = 50
console.log('copy: ', copy)
```
   
<h3>✅ 결과</h3>
```
original : {name: '홍길동', info: {age: 30}}
copy : {name: '김철수', info: {age: 50}}
```
   
### 깊은 복사 만드는 방법
1. `JSON.stringfy()` + `JSON.parse()`
2. `structureClone()`
   

## 얕은 복사 vs 깊은 복사 활용 사례
### 얕은 복사 활용 사례
- 최상위 속성만 변경이 필요한 경우
- 성능이 중요한 경우 (깊은 복사보다 빠름)
- 중첩된 객체가 없거나 변경할 필요가 없는 경우
   
### 깊은 복사 활용 사례
- 원본 데이터를 보존하면서 여러 변형을 시도해야할 때
- 복잡한 객체 구조에서 독립적인 사본이 필요할 때
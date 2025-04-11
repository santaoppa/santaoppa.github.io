---
title: "[JavaScript] var, let, const 차이"

categories:
  - Web
tags:
  - [Web, JavaScript]

toc: true
toc_sticky: true

date: 2025-04-10
last_modified_at: 2025-04-10
---

## var, let, const란?
var, let, const는 자바스크립트에서 변수를 선언할 때 사용하는 키워드이다.  

## var, let, const 차이
### 중복 선언과 재할당
- **var** : 중복 선언과 재할당 모두 가능
- **let** : 중복 선언 ❌ / 재할당 ⭕  
```js
let a = 5;
let a = 10;
cnosole.log(a); // SyntaxError: Identifier 'a' has already been declared
```
- **const** : 중복 선언 ❌ / 재할당 ❌  
```js
const a = 5;
console.log(a); // 5

a = 10;
console.log(a); // TypeError: Assignment to constant variable.
```  


### 스코프 범위
- **var** : 함수 단위 스코프  
```js
function test(){
    var a = 10;
    console.log(a);
}

test(); // 10
console.log(a);  // ReferenceError: a is not defined
```  

- **let** : 블록 단위 스코프  
- **const** : 블록 단위 스코프  
> {} 블록 내부에서 선언된 let, const 변수는 외부에서 참조되지 않는다.
```js
function test(){
    let a = 10;
    console.log(a);
}

console.log(a);  // ReferenceError: a is not defined
```  


### 호이스팅
- **var** : 호이스팅 ⭕  
```js
console.log(a);  // undefined : 변수 선언 이전에 변수 참조 가능

var a;  // 초기화

console.log(a);  // undefined

a = 10;  // 할당

console.log(a);  // 10
```  

- **let** : 호이스팅 ⭕
- **const** : 호이스팅 ⭕  
```js
console.log(a);  // ReferenceError : 변수 선언 이전에 변수 참조 불가능

let a;  // 초기화

console.log(a);  // undefined

a = 10;  // 할당

consloe.log(a);  // 10
```  


## ✅ 정리
- **var** : 되도록 사용 ❌ (예측 어려움)
- **let** : 변할 수 있는 값을 선언할 때 사용
- **const** : 변하지 않는 값을 선언할 때 사용 (Default)

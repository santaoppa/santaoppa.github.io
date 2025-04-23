---
title: "[React] 컴포넌트란?"
excerpt: "React에서 UI를 구성하는 핵심 단위"

categories:
  - React
tags:
  - [Web, React]

toc: true
toc_sticky: true

date: 2025-04-22
last_modified_at: 2025-04-22
---

## 컴포넌트란?
컴포넌트(Component)는 UI(User Interface)를 구성하는 **독립적이고 재사용 가능한 단위**
쉽게 말하면, 리액트는 컴포넌트들로 이루어진 애플리케이션임
  
## 컴포넌트 분리 기준
1. **재사용성**  
   여러 곳에서 반복적으로 사용되는 공통 UI는 별도의 컴포넌트로 분리하여 재사용할 수 있음

2. **단일 책임 원칙(SRP: Single Responsibility Principle)**  
   컴포넌트는 하나의 역할만 수행해야 하며, 기능이 복잡해지면 더 작은 컴포넌트로 나누는 것이 좋음

## 컴포넌트 선언
### 함수 표현식
변수에 함수를 할당하는 방식이며, 보통 **화살표 함수(Arrow Function)**를 사용함

이 방식으로 정의된 함수는 **할당된 이후에만 호출**할 수 있음!
  
```js
const myFunction = function() {};
```

➡️ 화살표 함수 문법
```js
const myFunction = () => {};
```

### 함수 선언식
`function` 키워드와 함수 이름을 사용하여 함수를 정의하는 방식

코드가 실행되기 전에 자바스크립트 엔진에 의해 **호이스팅**(함수 선언이 코드의 상단으로 끌어올려짐)이 발생하여 함수 선언 이전에 해당 함수를 호출할 수 있음

```js
console.log(hello()); // "Hello, world!"

function hello() {
  return "Hello, world!";
}
```
  
### 함수 표현식 vs 함수 선언식
|항목|화살표 함수|함수 선언식|
|------|---|---|
|**this 바인딩**|`this`가 자동으로 상위 스코프에 바인딩됨|`this`가 함수 내에서 새로 바인딩됨|
|**사용 사례**|함수형 컴포넌트 정의 시 주로 사용|일반적인 함수 정의 및 클래스형 컴포넌트 메서드 정의 시 사용|
|**생성 시점**|실행 시점에 생성|파싱 시점에 생성|

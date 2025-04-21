---
title: "[Web] JavaScript 핵심 개념"
excerpt : "자바스크립트 핵심 개념 : DOM, BOM, script 로딩 방식, this 등"

categories:
  - Web
tags:
  - [Web, JavaScript]

toc: true
toc_sticky: true

date: 2025-04-16
last_modified_at: 2025-04-16
---

## 문서 객체 모델 (Document Object Model, DOM)
HTML, XML, CSS 등을 트리 구조로 인식하여 각 요소를 객체로 간주하고 조작할 수 있게 해주는 인터페이스

## 브라우저 객체 모델 (Browser Object Model, BOM)
브라우저 창과 관련된 모든 객체 요소를 포함하는 모델로, window 객체를 중심으로 동작

### 주요 BOM 객체
- `window` : 모든 객체가 소속된 객체이며 **브라우저 창**
- `document` : 현재 문서에 대한 정보를 갖고 있는 객체
- `history` : 현재의 브라우저가 접근했던 URL history를 제어
- `location` : 문서의 주소와 관련된 객체로 window 객체의 프로퍼티인 동시에 document의 프로퍼티로 이 객체를 이용하여 윈도우의 문서 URL을 변경할 수 있고, 문서의 위치와 관련해서 다양한 정보를 얻을 수 있다.
>✅ window.location <br>
>✅ document.location
  

## script 로드 해결방법
### 1.body 최 하단에서 script 로드
HTML 파싱을 한 후에 script 태그를 로그할 수 있도록 body 태그 최하단으로 script 태그 위치를 이동
### 2.load 이벤트 리스너 등록
- `window.onload` : HTML 파싱 DOM 생성 그리고 외부 콘텐츠(images, script, css, etc)가 로드된 후 발생하는 이벤트
- `DOMContentLoaded` : HTML 파싱 DOM 생성 후 발생하는 이벤트  

## HTML5 script 로드 해결방법
`defer`, `async` 속성을 통해 비동기 script 로드 가능
HTML파싱과 함께 비동기로 JavaScript 파일을 불러옴  

### `defer` 속성
HTML 파싱이 완료된 후, JavaScript 코드 실행  

```js
  <script src="script.js" defer></script>
```

### `async` 속성
HTML 파싱이 완료되지 않았더라도, 먼저 로딩되는 JavaScript파일부터 실행이 시작됨.
JavaScript 파일을 실행할 때는 HTML 파싱이 중단됨  

```js
  <script src="script.js" async></script>
```

## this
객체를 가르키는 키워드 ~~호출한 놈(이 없을 경우에는 window 객체)~~

‼️ **예외**
1. **전역스코프**에서 this는 window 객체
2. **화살표 함수** : 자신이 포함하고 있는 외부 Scope에서 this를 계승받는다.
3.  **Strict Mode** : 엄격 모드에서는 기본값이 window가 아닌 undefined
   
```js
  console.log(this);  // default this => window

  function printThis() {
    console.log(this);
  }

  printThis();  // default this => window
```


```js
let person1 = {
   name: "짐코딩",
   age: 20,
   hello: function() {
      setTimeout(function() {
         console.log(this);
      }, 1000);
   }
};

person1.hello();  // window

let person2 = {
  name: "짐코딩",
  hello: function () {
    setTimeout(() => {
      console.log(this.name); 
    }, 1000);
  }
};

person2.hello(); // { name: '짐코딩', age: 20, hello: [Function: hello] }
```

```js
'use strict';

function printThis() {
  console.log(this);
}

printThis();  // undefined
```

### bind()란?
`bind()`는 함수의 `this`를 특정 객체로 고정시킨 새 함수를 반환하는 메서드
```js
const 바인딩된함수 = 원래함수.bind(this로 쓸 객체, [선택적 인자]);
```

‼️ bind는 한번만 사용 가능 (재지정 불가)
```js
function printThis() {
  console.log(this);  // default this => window
}
let person1 = {
  name: "홍길동",
};
let printThis1 = printThis.bind(person1);
printThis1(); // {name: '홍길동'}

let printThis2 = printThis.bind(person2);
printThis2(); // {name: '홍길동'}
```

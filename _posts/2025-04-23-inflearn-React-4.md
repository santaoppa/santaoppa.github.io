---
title: "[React] 이벤트 핸들링"
excerpt: ""

categories:
  - React
tags:
  - [Web, React]

toc: true
toc_sticky: true

date: 2025-04-23
last_modified_at: 2025-04-23
---

## 이벤트 핸들러란?
사용자의 **클릭**, **입력** 등과 같은 상호작용에 반응하여 실행되는 함수

## 이벤트 핸들러 추가하기
```js
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

- ✅ 인라인 함수로 정의
```js
<button onClick={function handleClick() {  alert('You clicked me!');}}>
```


- ✅ 화살표 함수로 정의
```js
<button onClick={() => {  alert('You clicked me!');}}>
```
  
# 이벤트 전파 멈추기
`e.stopPropagation()`은 이벤트가 상위 요소로 전파되는 것을 차단
```js
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}
```
  
# 기본 동작 방지하기
`e.preventDefault()` 는 기본 브라우저 동작을 가진 일부 이벤트가 해당 기본 동작을 실행하지 않도록 방지함
```js
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```
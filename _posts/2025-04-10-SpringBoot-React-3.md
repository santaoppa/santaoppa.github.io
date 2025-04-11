---
title: "[React] 게시판 프로젝트 - (3) React와 API 연동"

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-10
last_modified_at: 2025-04-10
---

## Axios 설치
```
npm i axios
```
> **Axios란?** jQuery의 Ajax와 비슷한 라이브러리로  node.js와 브라우저를 위한 Promise 기반 HTTP 클라이언트  



## 환경 변수 파일 생성
react 프로젝트에 .env 파일 생성
```
REACT_APP_API_URL=http://localhost:8080/api
```

> 💡 **주의할 점** <br/>
> 1. 환경 변수 이름은 반드시 `REACT_APP_` 으로 시작해야함<br/>
> 2. `.env` 파일 수정 후에는 서버 재시작 필요  



## 게시글 작성
API 요청 URL을 환경변수에서 불러와 axios를 이용해 
Spring boot API 서버에 POST 요청을 보냄

```js
const onClickSubmit = () => {
    const apiUrl = `${process.env.REACT_APP_API_URL}/posts`;

    axios.post(apiUrl, formData)

        // 요청이 성공했을 때 실행되는 부분
        .then((response) => {
            // 서버에서 응답으로 받은 데이터 콘솔에 출력
            console.log('✅ 게시글 등록 성공:', response.data);
        })

        // 요청이 실패했을 때 실행되는 부분
        .catch((error) => {
            // 에러 내용을 콘솔에 출력
            console.error('❌ 게시글 등록 실패:', error);
        });
};
```

React와 API 연동하여 게시글 등록 성공!

![Image](https://github.com/user-attachments/assets/97c9e603-36a0-424e-a610-920f882565e7)  




## 게시글 수정
React는 상태 기반 UI를 지향하기 때문에 폼 입력값도 `useState`로 상태를 선언하고, 이를 통해 입력값을 제어하는 것이 기본 방식임

🔍 React는 단방향 데이터 흐름을 따르므로, input 요소에 value를 지정하면 해당 값은 반드시 **상태(state)**에서 제공되어야 하며, 사용자의 입력에 따라 상태를 변경하기 위해 `onChange`를 통해 `setPost()` 등의 상태 업데이트 함수가 반드시 필요

이처럼 value가 상태로 제어되는 input 요소를 **제어 컴포넌트(controlled component)**라고 부름

상태를 통해 값을 직접 관리하지 않고 DOM에서 값을 가져오는 방식은 HTML의 전통적인 방식이며, `e.target.title.value`와 같이 submit 시에 직접 폼 값을 추출하는 방식은 React 스타일이 아님

```js
const handleChange = (e) => {
    const { name, value } = e.target;
    setPost(prevPost => ({
        ...prevPost,
        [name]: value
    }));
};
 
 ...

<div className="form-group">
    <label htmlFor="title">제목</label>
    <input
        id="title"
        name="title"
        type="text"
        value={post.title}
        onChange={handleChange}
        placeholder="제목을 입력하세요"
        required
    />
</div>
<div className="form-group">
    <label htmlFor="content">내용</label>
    <textarea
        id="content"
        name="content"
        value={post.content}
        onChange={handleChange}
        placeholder="내용을 입력하세요"
        required
    />
</div>
```

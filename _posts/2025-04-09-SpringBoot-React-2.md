---
title: "[React] 게시판 프로젝트 - (2) Router 컴포넌트 구현"
excerpt: "게시판 관련 컴포넌트 및 Router 컴포넌트 구현 "

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-09
last_modified_at: 2025-04-09
---

> **Routing이란?** 사용자가 요청한 URL에 따라 해당 URL에 맞는 페이지를 보여주는 것

## React Router란?
React에서 URL에 따른 컴포넌트 렌더링을 관리하기 위한 라이브러리를 말한다.  

> 💡React는 SPA(Single Page Application)으로 하나의 페이지를 지원하는 Application이므로 Routing 기능을 지원하는 라이브러리를 설치해 사용해야함  

A태그는 화면을 새로고침한 다음에 페이지를 이동하지만, React Router에서는 새로운 페이지를 로드하지 않고 하나의 페이지 안에서 필요한 데이터만 가져오기 때문에 불필요한 렌더링을 막을 수 있다.  


### BrowserRouter란?
`BrowserRouter`는 브라우저 주소가 변경되면 해당 주소에 대응하는 뷰를 렌더링해준다.
`BrowserRouter`는 React Router DOM을 적용하고 싶은 컴포넌트의 최상위 컴포넌트를 감싸주는 래퍼 컴포넌트이다.  


### Routes
여러 개의 `Route` 컴포넌트를 그룹화하고 관리하기 위한 컴포넌트로
경로가 일치하는 단 하나의 `Route`의 `element` `prop`으로 지정한 컴포넌트트만 렌더링 시켜줌  


### Route
실제로 주어진 경로에 대한 컴포넌트를 렌더링하기 위한 컴포넌트
`<Route>` `path` 속성에 경로, `element` 속성에 보여주고자 하는 컴포넌트를 넣어준다.  



## React Router 구현 (페이지 처리)
- react-router 라이브러리 설치
```
npm install react-router-dom
```  

### 예시 코드
```js
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import BbsWrite from "./pages/BbsWrite";
import BbsDetail from "./pages/BbsDetail";
import BbsEdit from "./pages/BbsEdit";

function App() {
    return (
        <Router>
            <div className="App">
                <Routes>
                    <Route path="/" element={<Home />} />
                    <Route path="/write" element={<BbsWrite />} />
                    <Route path="/post/:id" element={<BbsDetail />} />
                    <Route path="/post/edit/:id" element={<BbsEdit />} />
                </Routes>
            </div>
        </Router>
    );
}

export default App;
```


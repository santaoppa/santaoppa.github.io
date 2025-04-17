---
title: "[React] 게시판 프로젝트 - (7) 공통 API 요청 처리 인스턴스 생성"
excerpt: "공통 API 요청 처리 인스턴스를 활용하여 동기화된 토큰 정보를 API 요청에 포함시킬 수 있음"

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## ‼️ 문제점
매 요청마다 localStorage에서 토큰 값을 가져와서 Axios 인스턴스의 header에 주입해줌.
그런데 `UserApi`에만 토큰을 넣어주니, 다른 컴포넌트에서 Axios로 요청을 보낼 때는 백엔드에서 토큰 값을 확인할 수 없음!  
  
### 기존 코드
```js
export const UserApi = axios.create({
    baseURL: 'http://localhost:8080',
    headers: {
        'Content-Type': 'application/json',
    },
});

UserApi.interceptors.request.use((config) => {
    const accessToken = localStorage.getItem("accessToken");
    const refreshToken = localStorage.getItem("refreshToken");

    if (accessToken) config.headers['Authorization'] = `Bearer ${accessToken}`;
    if (refreshToken) config.headers['Refresh-Token'] = refreshToken;

    return config;
}, (error) => {
    return Promise.reject(error);
});
```

## ☑️ 해결책
토큰을 모든 요청의 header에 자동으로 추가하기 위해 `AxiosApi`라는 **공통 API 요청 처리 인스턴스를 생성**하여
여러 컴포넌트에서 토큰을 쉽게 관리하고, API 요청을 처리할 수 있음

### 1. 공통 API 요청 처리 인스턴스 생성  
```js
const AxiosApi = axios.create({
    baseURL: process.env.REACT_APP_API_URL,  // 기본 API URL
})

// 요청 인터셉터 설정: 매 요청마다 Authorization 헤더에 Bearer 토큰 추가
AxiosApi.interceptors.request.use((config) => {
    const accessToken = localStorage.getItem("accessToken");
    const refreshToken = localStorage.getItem("refreshToken");

    if (accessToken) config.headers['Authorization'] = `Bearer ${accessToken}`;
    if (refreshToken) config.headers['Refresh-Token'] = refreshToken;

    return config;
}, (error) => {
    return Promise.reject(error);
});
```
### 2. 토큰 갱신
추후 토큰 갱신이 필요할 때, 이 공통 인스턴스를 통해 `AxiosApi` 설정을 업데이트하여 
다음 요청에 새로운 토큰을 반영할 수 있음
```js
const refreshAccessToken = async () => {
    const response = await AxiosApi.get(`/auth/refresh`);
    const newAccessToken = response.data.accessToken;
    localStorage.setItem('accessToken', newAccessToken);

    // 새로운 토큰을 AxiosApi에 설정
    AxiosApi.defaults.headers['Authorization'] = `Bearer ${newAccessToken}`;
}
```

### 3. API 요청
공통 API 요청 처리 인스턴스를 사용하여, 동기화된 토큰 정보를 API 요청과 함께 보낼 수 있음
```js
export const fetchUser = async () => {
    const response = await AxiosApi.get(`/user`);
    return response.data;
}
```
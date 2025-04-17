---
title: "[React] 게시판 프로젝트 - (6) JWT 인증 구현(react)"
excerpt: Spring Boot에서 구현한 JWT 기반 인증 시스템을 React 클라이언트에서 사용하는 방법

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## 로그인 처리
Spring 서버에서 로그인 후, 응답 헤더에 포함된 accessToken과 refreshToken을 localStorage에 key-value 형태로 저장함  

```js
const handleSubmit = (e) => {
    e.preventDefault(); // 기본 submit 동작 방지
    login(values)
        .then((response) => {
            localStorage.clear();

            const accessToken = response.headers['authorization'];
            const refreshToken = response.headers['refresh-token'];

            localStorage.setItem("accessToken", accessToken);
            localStorage.setItem("refreshToken", refreshToken);

            // Axios 기본값으로 Authorization 헤더 세팅
            axios.defaults.headers.common['Authorization'] = accessToken;

            window.location.href = `/`;
        })
        .catch((error) => {
            console.log(error);
        });
};
```

## Axios 인스턴스 설정
매 요청마다 localStorage에서 토큰값을 가져와 Axios 인스턴스 header에 주입  

```js
// AXIOS 인스턴스 생성
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

> ⚠️ 백엔드에서는 토큰을 응답 헤더에 Authorization, Refresh-Token으로 설정하고, Bearer 접두사는 프론트에서 붙이는 것이 일반적  


## 토큰 유효성 검증 및 재발급
interceptor를 통해 Axios 인스턴스가 요청의 응답을 가로채 access Token의 유효성을 검사함
에러코드가 `403`인 경우 refresh token을 통해 access token의 재발급을 수행

> `401` : 로그인하지 않은 상태<br>
> `403` : 로그인하였으나 `AccessToken`이 유효하지 않은 상태

```js
// 토큰 유효성 검사
UserApi.interceptors.response.use((response) => {
    return response;
}, async (error) => {
    const originalRequest = error.config;
   
   if (error.response.status === 403 && !originalRequest._retry) {
        originalRequest._retry = true;

        try {
            await refreshAccessToken();
            return UserApi(originalRequest);
        } catch (refreshError) {
            console.error("토큰 갱신 실패", refreshError);
            forceLogout();
        }
    }

    return Promise.reject(error);
});

// 토큰 갱신
const refreshAccessToken = async () => {
    const response = await UserApi.get(`/api/auth/refresh`);
    const newAccessToken = response.data;
    localStorage.setItem('accessToken', newAccessToken);

    // 다음 요청부터 사용할 수 있도록 전역 axios 설정 업데이트
    UserApi.defaults.headers.common['Authorization'] = `Bearer ${ACCESS_TOKEN}`;
}
```
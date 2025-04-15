---
title: "[Web] Bearer이란?"

categories:
  - Web
tags:
  - [Web]

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## Authorization란?
클라이언트의 인증 정보를 서버에 전달하는 헤더로 서버는 이 헤더를 이용해 요청을 보낸 사용자가 누구인지 확인하고,
해당 사용자가 해당 리소스에 접근할 수 있는 권한이 있는지를 판단함

```
Authorization: <type> <credentials>
```
- `type` : 인증 방식 (ex. Basic, Bearer, Digest 등)
- `credentials` : 인증 정보 (ex. 토큰, 암호화된 문자열 등등)

## Bearer란?
Bearer는 Authorization 헤더에서 사용하는 인증 방식 중 하나로,
**“이 토큰을 가진 자(Bearer)가 권한을 가진 것으로 간주하겠다”**는 의미에서 붙은 이름

```
Authorization: Bearer <token>
```


## Bearer와 JWT 차이
Bearer는 HTTP 인증 방식 중 하나이고,
JWT는 인증에 사용되는 토큰 구조 중 하나임

✅ JWT는 Bearer에서 사용되는 토큰의 종류임
<img width="512" alt="Image" src="https://github.com/user-attachments/assets/675c1837-6361-4691-b08e-3f664cb00401" />


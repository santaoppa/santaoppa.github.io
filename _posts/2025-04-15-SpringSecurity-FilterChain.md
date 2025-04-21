---
title: "[Spring Security] Filter Chain이란?"

categories:
  - Web
tags:
  - [Web, Spring Security]

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## Filter란?
클라이언트가 서버에 요청을 보내거나, 서버가 클라이언트에게 응답을 보내기 전에 가장 먼저 실행되는 Java 클래스

HTTP 요청이 들어오면 서블릿(Servlet)까지 도달하기 전에 
1) 먼저 `doFilter()` 메서드를 통해 요청을 가로채고 
2) 필요한 로직을 실행한 뒤, 
3) 요청을 다음 목적지(필터 또는 서블릿)로 전달  


## Filter chain이란?
여러 개의 필터가 체인처럼 연결되어 하나의 요청에 대해 순차적으로 실행되는 구조로로

클라이언트로부터 HTTP 요청을 받으면, `DispatcherServlet`까지 가기 전에 여러 개의 필터가 순차적으로 실행됨

각 필터는 다음 필터로 요청을 넘기기 위해 `chain.doFilter()`를 호출하며, 이 구조를 "Filter Chain"이라고 부름

![Image](https://github.com/user-attachments/assets/176cef72-43f1-4be3-aa9d-1750bb4e4a17)
클라이언트가 API 요청을 하면 Web Server ➡️ Servlet ➡️ Controller 순으로 요청이 전달되는데,
Filter Chain은 Web Server와 Servlet 사이에서 작동한다.  

## Spring Security의 Filter
Spring Security는 주요 보안에 대한 처리를 여러가지 필터로 처리하도록 구성함 
- `UsernamePasswordAuthenticationFilter` : 로그인 요청(/login) 시 ID/PW를 검증  
- `ExceptionTranslationFilter` : 인증/인가 예외 처리  

## ✅ 정리
- Filter는 HTTP 요청을 가장 먼저 가로채는 컴포넌트이며,
Spring Security는 이 필터 구조를 활용해 인증/인가, 보안 처리 등을 수행함  
- 필터는 **체인(FilterChain)**을 통해 순서대로 실행되고,  
필요에 따라 요청을 가로채거나, 다음 필터/서블릿으로 넘겨줌  
- Spring Security에서의 모든 인증 흐름(JWT, 로그인, 권한 등)은 결국 필터 기반으로 처리됨

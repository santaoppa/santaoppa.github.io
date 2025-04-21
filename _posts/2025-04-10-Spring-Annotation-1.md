---
title: "[Spring] @RestController란?"

categories:
  - Web
tags:
  - [Spring]

toc: true
toc_sticky: true

date: 2025-04-10
last_modified_at: 2025-04-10
---

## `@RestController`란?
Controller는 View 페이지뿐만 아니라 Data 반환하는 역할도 수행할 수 있다. 이때 데이터를 응답으로 제공하려면, 컨트롤러에 `@RestController` 어노테이션을 사용해야 한다.

`@RestController`는 `@Controller`에 `@ResponseBody`가 결합된 어노테이션으로, 메서드의 반환값을 HTTP 응답 본문에 직접 담아 JSON 형태로 클라이언트에 전달한다.

데이터를 응답으로 제공하는 REST API를 개발할 때 주로 사용되며, 일반적으로 반환 객체는 `ResponseEntity`로 감싸서 응답 상태 코드와 함께 전달한다.  



## `@RestController` 동작방식
![Image](https://github.com/user-attachments/assets/b5ebcb79-4049-4d47-9378-b162b7237e69)

1. Client는 URL 형식으로 요청을 보낸다.
2. DispatcherServlet이 요청을 위임할 Handler Mapping을 찾는다.
3. Handler Mapping을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후 객체를 반환한다.
5. 반환되는 객체는 JSON으로 Serialize되어어 Client에게 반환된다.

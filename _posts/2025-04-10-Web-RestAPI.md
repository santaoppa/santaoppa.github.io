---
title: "[Web] Rest API란?"

categories:
  - Web
tags:
  - [Web]

toc: true
toc_sticky: true

date: 2025-04-10
last_modified_at: 2025-04-10
---

![Image](https://github.com/user-attachments/assets/ccdfe5aa-1930-43c1-9120-f31a5f10be7a)

## Rest(Representational State Transfer)란?
자원을 이름(URI)으로 구분하고, 해당 자원의 상태를 주고받는 모든 행위를 의미함

1. HTTP URI을 통해 자원(Resource)을 명시하고
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원에 대한 CRUD 연산을 수행하는 방식

즉, **URI**를 통해 요청할 자원을 지정하고, GET, POST 등의 **방식**을 사용하여 원하는 작업(CRUD)를 수행하며, 요청을 위한 자원은 특정한 형태로 **표현**하는 것을 의미

> **URL(Uniform Resource Locator)이란?** 인터넷 상에서 자원의 위치를 의미
> **URI(Uniform Resource Identifier)이란?** 인터넷 상의 자원을 식별하기 위한 문자열의 구성으로, URI는 URL을 포함  

 
### Rest 구성 요소
1. **자원(Resource)** → HTTP URI
2. **행위(Verb)** → HTTP Method (GET, POST, PUT, DELETE, PATCH 등)
3. **표현(Representation)** → HTTP 메시지의 Body (JSON, XML, TEXT 등)
> 클라이언트가 자원의 상태에 대한 조작을 요청하면 서버는 이에 적절한 응답을 보낸다. 이때 데이터를 주고받는 형태로 JSON, XML, TEXT, RSS 등 다양한 Representation으로 나타낼 수 있다.  


### Rest의 특징
1. **서버-클라이언트 구조**
클라이언트는 유저와 관련된 처리를, 서버는 REST API를 제공함으로써 각각의 역활이 확실하게 구분되고 일괄적인 인터페이스로 분리되어 작동할 수 있게 한다.
- 서버는 API를 제공하고 비지니스 로직 처리 및 저장을 책임
- 클라이언트는 사용자 인증이나 context(세션, 로그인 정보)등을 직접 관리하고 책임  

2. **무상태**
HTTP 프로토콜이 무상태이므로, REST 또한 요청 간 상태를 저장하지 않음
- 서버는 각 요청을 독립적으로 처리  

3. **캐시 처리 가능**
웹 표준을 따르기 때문에, 이미지, 페이지, 스크립트 등은 캐시를 통해 성능 향상    

4. **계층화**
클라이언트와 서버 사이에 프록시, 로드 밸런서, 보안 계층 등 중간 계층을 두어 확장 가능  

5. **인터페이스 일관성**
URI로 자원을 지정하고, 제한된 HTTP Method로 자원에 대한 행위를 수행  



## Rest API란?
Rest의 특징을 기반으로 서비스 api를 구현한 것으로 
요청만 봐도 어떤 동작과 목적을 가진 API인지 명확하게 추론 가능함

1. URI는 정보의 자원을 표현해야 함
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, PATCH, DELETE)로 표현해야함

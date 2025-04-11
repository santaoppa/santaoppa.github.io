---
title: "[React] 게시판 프로젝트 - (1) Spring Boot와 React 연동하기"
excerpt: "Spring Boot + React.js 개발환경 구축하기 "

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-08
last_modified_at: 2025-04-08
---

## 개발환경

> **OS**: Windows <br>
> **IDE**: IntelliJ IDEA <br>
> **BackEnd**: Spring Boot 3.4.4 <br>
> **FrontEnd**: React, JavaScript <br>
> **Language**: Java 17.0.9 <br>
> **Build**: Maven <br>
> **Database**: PostgreSQL  



## React 프로젝트 생성
사전에 생성해준 Spring Boot 내부의 src/main 폴더 아래 React 프로젝트를 생성
```
cd src/main
npx create-react-app frontend
```  



## React 실행
```
cd frontend
npx start
```  



## Proxy 설정
CORS 문제를 방지하기 위해 Proxy 설정이 필요함

- **React** : `package.json` 파일에 proxy 값을 추가하여 Spring Boot 프로젝트의 포트를 작성해준다.
```
"proxy": "http://localhost:8080",
```  

- **Spring Boot** : Spring boot에서 config 패키지를 생성 -> `WebConfig.java` 추가
```java
@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")  // 모든 경로에 대해 적용
                        .allowedOrigins("http://localhost:3000")  // 허용할 출처 지정
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")  // 허용할 HTTP 메서드
                        .allowedHeaders("*")
                        .allowCredentials(true);
            }
        };
    }
}
```  


<details>
    <summary>CORS란?</summary>
    <div markdown="1">
        "Cross-Origin Resource Sharing"의 약자로, 웹 애플리케이션에서 발생하는 보안 정책이다. 이 정책은 웹 페이지가 다른 출처의 자원을 요청하는 것을 제한하는 보안 메커니즘으로 프로토콜, 도메인, 포트 번호 중 하나라도 다를 경우 교차 출처가 된다. 현재 스프링 부트가 실행되는 포트 번호 8080과 React가 실행되는 포트 번호 3000이 서로 다르기 때문에 교차 출처가 되어 브라우저는 보안 상의 이유로 HTTP 요청을 제한하게 된다. 백엔드에서 CORS 설정을 해주거나 프론트엔드에서 proxy 값을 설정하여 CORS 문제를 회피할 수 있다.
    </div>
</details>  



## Spring Boot와 React 서버 API 통신
React 애플리케이션은 백엔드(Spring Boot)에서 정의한 REST API 엔드포인트를 호출해 필요한 데이터를 가져오며, 응답받은 데이터를 기반으로 화면에 동적으로 표시함  

fetch와 axios 둘다 HTTP 요청을 처리하는 JavaScript 라이브러리로, 네트워크 요청을 보내고 응답을 받아오는 기능을 제공한다.

> **fetch란?**
 브라우저에 기본 내장된 네이티브 API로 브라우저 환경에서 주로 사용된다. 이를 통해 비동기적으로 네트워크 요청을 수행하고 응답을 처리할 수 있다. fetch는 기본적으로 CSRF(Cross-Site Request Forgery) 보호를 제공하지 않으며, JSON 데이터를 자동으로 파싱해주지 않는다. 따라서 응답을 처리할 때 수동으로 response.json() 메서드를 사용하여 JSON 데이터를 파싱해야 한다.  
{: .prompt-tip }

> **axios란?**
 브라우저와 Node.js 환경에서 모두 사용할 수 있는 Promise 기반의 HTTP 클라이언트 라이브러리이다. axios는 네트워크 요청과 응답을 다루는데 유연성과 편의성을 제공한다. axios는 기본적으로 CSRF 보호를 제공하고 있으며, JSON 데이터를 자동으로 파싱하여 JavaScript 객체로 반환해준다.  
{: .prompt-tip }


### axios 라이브러리 설치
```
npm install axios --save
```  


### API 요청 코드 작성
- `TestController.java` : API 요청을 처리하는 백엔드 엔드포인트 구현

```java
package com.example.demoproject.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

@RestController
public class TestController {

    @GetMapping("/api")
    public String test(){
        return "안녕하세요. 현재 서버시간은 " + new Date() + "입니다. \n";
    }
}
```

- `src/main/frontend/App.js` : API를 호출하는 프론트엔드 로직 작성

```js
import {useEffect, useState} from "react";
import axios from "axios"

function App() {
    const [data, setData] = useState('')

    useEffect(() => {
        axios.get('/api')
            .then(res => setData(res.data))
            .catch(err => console.log(err))
    }, []);

    return (
        <div>
            {data}
        </div>
    )
}

export default App;
```

localhost:3000에서 localhost:8080/api로부터 데이터를 요청 받아와 출력되는 것 확인 가능함!

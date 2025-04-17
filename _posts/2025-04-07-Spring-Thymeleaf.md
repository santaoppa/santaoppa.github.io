---
title: "[Spring Boot] 타임리프(Thymeleaf)란?"

categories:
  - Spring
tags:
  - [Spring boot, Thymeleaf]

toc: true
toc_sticky: true

date: 2025-04-07
last_modified_at: 2025-04-07
---

타임리프(Thymeleaf)는 Spring MVC 애플리케이션에서 JSP를 대체할 수 있는 템플릿 엔진으로, Spring Boot 환경에서는 JSP보다 타임리프 사용을 권장하고 있음  


Spring Boot는 기본적으로 `src/main/resources/templates` 디렉토리를 템플릿 파일의 기본 경로로 인식하며, 타임리프는 해당 경로에서 동작할 수 있도록 자동 설정됨  

반면, JSP는 Servlet 컨테이너에서 동작해야 하므로, Spring Boot에서 JSP를 사용하려면 별도의 설정이 필요함  

또한 타임리프 프로젝트는 JAR 패키징이 가능하지만, JSP는 WAR 패키징만 지원함
WAR는 웹서버(WAS)에서 실행되며, WEB-INF 등 사전 정의된 디렉토리 구조를 따라야 함  

이로 인해 Spring Boot의 장점인 간편한 JAR 실행 및 내장 서버 사용이 제한됨  


결론적으로, Spring Boot 기반 애플리케이션에서는 JSP보다 *타임리프를 사용하는 것*이 구조적으로 더 적합하며, 빌드, 배포, 유지보수 측면에서도 효율적임!
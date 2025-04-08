---
title: "[Spring] @Bean과 @Component 차이"

categories:
  - Spring
tags:
  - [Spring]

toc: true
toc_sticky: true

date: 2025-04-08
last_modified_at: 2025-04-08
---

`@Bean` : 메소드 레벨에서 선언하며, 반환되는 객체(인스턴스)를 개발자가 수동으로 빈으로 등록하는 어노테이션으로
개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 직접 등록하고 싶은 경우에 사용된다.

`@Component` : 클래스 레벨에서 선언하며 싱글톤 클래스 빈을 생성하는 어노테이션으로 
내부에서 직접 접근 가능한 클래스에 사용하면 된다.
---
title: "[Spring] @Autowired란?"

categories:
  - Web
tags:
  - [Spring]

toc: true
toc_sticky: true

date: 2025-04-10
last_modified_at: 2025-04-10
---

## `@Autowired`란?
스프링에서 의존성 주입(DI)을 자동으로 처리해주는 어노테이션으로 스프링 컨테이너에 등록된 빈 중에 필요한 객체를 찾아 자동으로 주입해줌  


스프링은 내부적으로 모든 빈을 컨테이너에 먼저 등록한 후에, 의존성 주입 단계에서 `@Autowired` 어노테이션이 부여된 대상에 해당 빈을 주입함. 이 어노테이션이 적용된 생성자, 메서드, 필드에 적절한 빈이 자동으로 연결되면서 의존관계가 설정됨!  


`@Autowired`는 총 세 가지 방법(생성자, 수정자, 필드)으로 실현이 가능함
> [의존성 주입 참고](https://santaoppa.github.io/spring/Spring-DI/)
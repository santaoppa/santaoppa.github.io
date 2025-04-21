---
title: "[Spring] AOP(Aspect-Oriented Programming)란?"

categories:
  - Spring
tags:
  - [Spring]

toc: true
toc_sticky: true

date: 2025-04-08
last_modified_at: 2025-04-08
---

## 관점 지향 프로그래밍(Aspect-Oriented Programming, AOP)이란?
여러 개의 클래스에서 반복해서 사용하는 코드가 있다면 해당 코드를 모듈화하여 공통 관심사로 분리한다.
이렇게 분리한 공통 관심사를 Aspect로 정의하고, Aspect를 적용할 메소드나 클래스에 Advice를 적용하여 공통 관심사와 핵심 관심사를 분리할 수 있다.

> **Advice란?** Aspect(공통적인 기능을 모듈화 한 것)의 기능을 정의한 것
{: .prompt-tip }  

## Spring AOP란?
Spring AOP는 스프링 프레임워크에서 제공하는 기능 중 하나로 관점 지향 프로그래밍을 지원하는 기술이다.  
로깅, 보안, 트랜잭션 관리 등과 같은 공통적인 관심사를 모듈화하여 코드 중복을 줄이고 유지 보수성을 향상하는데 도움을 준다.  

단, 스프링 빈에만 AOP를 적용할 수 있다.

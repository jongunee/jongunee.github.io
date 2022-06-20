---
layout: post
title: BeanFactory와 ApplicationContext
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# BeanFactory와 ApplicationContext

* toc
{:toc .large-only}

## BeanFactory와 ApplicationContext 상속 관계

![그림1](/assets/img/spring/beanfactory_heritance_relationship.png)

## BeanFactory

- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈 관리 및조회
- `getBean()` 제공

## ApplicationContext

- `BeanFactory` 모든 기능 상속받아 제공
- 빈 관리 조회 외에도 수 많은 부가기능 제공
- `ApplicationContext`가 부가기능에 `BeanFactory`에 기능도 포함하기 때문에 `BeanFacotry`는 거의 사용하지 않고 `ApplicationContext`를 사용

![그림2](/assets/img/spring/application_context_function.png)

__ApplicationContext 부가기능__

- `MessageSource`: 메시지 소스를 활용한 국제화 기능  
    ex) 국가에 따라 언어를 바꿔서 출력할 때
- `EnvironmentCapable`: 환경변수  
    ex) 환경(로컬, 개발, 운영 등)에 따라 정보를 달리 처리할 때
- `ApplicationEventPublisher`: 애플리케이션 이벤트  
    ex) 이벤트 발행 및 구독 모델 조회
- `ResourceLoader`: 편리한 리소스 조회  
    ex) 파일, 클래스, 외부 등에스 리소스 조회


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

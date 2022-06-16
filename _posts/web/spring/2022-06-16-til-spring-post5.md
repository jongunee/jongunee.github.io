---
layout: post
title: 자바 코드 스프링 전환
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 자바 코드 스프링 전환

* toc
{:toc .large-only}

## AppConfig 스프링 기반으로 변경

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }
    ...
}
```
- 각 메서드에 어노테이션 추가
  - __@Configuration:__ AppConfig에 설정 구성
  - __@Bean:__ 스프링 컨테이너에 스프링 빈으로 등록

## MemberApp 스프링 컨테이너 적용

```java
public class MemberApp {
    public static void main(String[] args) {
//        AppConfig appConfig = new AppConfig();
//        MemberService memberService = appConfig.memberService();
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        
        ...

    }
}
```
- ApplicationContext라는 스프링 컨테이너 사용해서 DI, 객체 관리
- `AnnotationConfigApplicationContext`: AppConfig에 있는 환경 설정 정보를 가지고 스프링이 어노테이션을 찾아 객체 생성 및 관리
- `@Bean`이 붙은 모든 메서드 호출해서 반환된 객체를 스프링 컨테이너에 등록
- `applicationContext.getBean("memberService", MemberService.class)`: `@Bean`이 붙은 메서드 중 `memerService` 이름을 갖고 `MemberService` 타입인 객체를 가져온다는 의미

## OrderApp 스프링 컨테이너 적용

```java
public class OrderApp {
    public static void main(String[] args) {

//        AppConfig appConfig = new AppConfig();
//        MemberService memberService = appConfig.memberService();
//        OrderService orderService = appConfig.orderService();

        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
 
        ...

    }
}
```
MemberApp과 동일


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

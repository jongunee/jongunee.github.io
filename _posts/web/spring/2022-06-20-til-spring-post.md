---
layout: post
title: 스프링 컨테이너와 스프링 빈
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 컨테이너와 스프링 빈

* toc
{:toc .large-only}

## 스프링 컨테이너

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
```
- `ApplicationContext`가 스프링 컨테이너이자 인터페이스
- xml 기반의 스프링 컨테이너로 만들 수도 있지만 요즘은 잘 사용하지 않음
- 스프링 컨테이너는 `BeanFactory`와 `ApplicationContext`로 구분해서 말하는데 `BeanFactory`를 직접 사용하는 경우는 거의 없기 떄문에 일반적으로 `ApplicationContext`를 스프링 컨테이너라 한다.

## 스프링 컨테이너 생성 과정

![그림1](/assets/img/spring/spring_container_creation.png)

__스프링 컨테이너 생성__

1. `new AnnotationConfigApplicationContext(AppConfig.class)`: `AppConfig.class`의 정보 전달
    - 스프링 컨테이너 생성
    - 스프링 컨테이너 안 스프링 빈 저장소 생성
        - key: 빈 이름
        - value: 빈 객체
2. 스구성 정보 활용
    - `AppConfig` 같은 구성 정보가 전달 되어야 함
    - 구성 정보를 보고 객체 등 생성

__스프링 빈 등록__

![그림2](/assets/img/spring/spring_bean_registration.png)

1. `AppConfig` 설정 정보를 확인해서 애너테이션`@Bean`이 붙어 있는 메서드 호출
2. 해당하는 메서드를 호출해서 빈 객체로 등록
    - 빈 이름(Key): 메서드 이름
    - 빈 객체(Value): 메서드가 호출되고 리턴된 객체
    - 빈 이름을 직접 부여할 수도 있음  
    ex) `@Bean(name="memberService2")`
    - 빈 이름을 같은 이름으로 부여하면 다른 빈이 무시되거나 기존 빈을 덮어버리기 때문에 <mark>빈 이름은 항상 다른 이름을 부여해야한다</mark> 

__스프링 빈 의존 관계 설정__

![그림3](/assets/img/spring/spring_bean_di_preparation.png)
스프링 빈 생성

![그림4](/assets/img/spring/spring_bean_di_completion.png)
스프링 빈에 의존 관계 대입 (DI)


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

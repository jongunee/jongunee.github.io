---
layout: post
title: 빈 스코프
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 빈 스코프

* toc
{:toc .large-only}

## 빈 스코프란?

> 빈이 생성되어 종료되는 라이프사이클 범위

__스프링 지원 스코프__

- __싱글톤:__  
기본 스코프, 스프링 컨테이너의 시작부터 종료까지 유지되는 가장 넓은 범위 스코프로 여태까지는 모두 싱글톤 스코프
- __프로토타입:__  
스프링 컨테이너가 프로토타입의 빈 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위 스코프
- 웹 관련 스코프:
  - `request`:  
  웹 요청이 들어오고 나갈때까지 유지되는 스코프
  - `session`:  
  웹 세션이 생성되고 종료될 때까지 유지되는 스코프
  - `application`:  
  웹의 서블릿 컨텍스와 같은 범위로 유지되는 스코프

## 스코프 선언

__컴포넌트 스캔 자동 등록__

```java
@Scope("prototype")
@Component
public class HelloBean {}
```

__컴포넌트 스캔 수동 등록__

```java
@Scope("prototype")
@Bean
PrototypeBean HelloBean() {
  return new HelloBean();
}
```

## 스코프 동작

__싱글톤 빈 요청__

![그림1](/assets/img/spring/singletone_bean_request.png)

1. 스프링 컨테이너에 싱글톤 스코프 빈 요청
2. 스프링 컨테이너가 관리하는 스프링 빈 반환
3. 같은 요청이 와도 같은 객체 인스턴스 스프링 빈 반환

![그림2](/assets/img/spring/prototype_bean_request1.png)

1. 스프링 컨테이너에 프로토타입 스코프 빈 요청
2. 요청시 스프링 컨테익너가 프로토타입 빈 생성 및 필요 의존관계 주입


![그림3](/assets/img/spring/prototype_bean_request2.png)

3. 스프링 컨테이너는 생성한 프로토타입 빈 반환
4. 이후 같은 요청이 와도 항상 새로운 프로토타입 빈 생성 및 반환 

스프링 컨테이너는 프로토타입 빈을 <span style='background-color: #f5f0ff'>생성, 의존관계 주입, 초기화까지만 처리</span>

## 싱글톤 빈 스코프 테스트

```java
public class SingletonTest {

    @Test
    void SingletonBeanFind(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);

        SingletonBean singletonBean1 = ac.getBean(SingletonBean.class);
        SingletonBean singletonBean2 = ac.getBean(SingletonBean.class);

        System.out.println("singletonBean1 = " + singletonBean1);
        System.out.println("singletonBean2 = " + singletonBean2);

        assertThat(singletonBean1).isSameAs(singletonBean2);

        ac.close();
    }

    @Scope("singleton")
    static class SingletonBean{
        @PostConstruct
        public void init(){
            System.out.println("SingletonBean.init");
        }

        @PreDestroy
        public void destroy(){
            System.out.println("SingletonBean.destroy");
        }
    }
}
```
- 스프링 컨테이너 생성 시점에 빈 초기화 메서드 실행
- 같은 인스턴스 빈 조회
- 종료 메서드 호출

## 프로토타입 빈 스코프 테스트

```java
public class PrototypeTest {

    @Test
    void prototypeBeanFind(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
        System.out.println("find prototypeBean1");
        PrototypeBean prototypeBean1= ac.getBean(PrototypeBean.class);

        System.out.println("find prototypeBean2");
        PrototypeBean prototypeBean2= ac.getBean(PrototypeBean.class);

        System.out.println("prototypeBean1 = " + prototypeBean1);
        System.out.println("prototypeBean2 = " + prototypeBean2);
        Assertions.assertThat(prototypeBean1).isNotSameAs(prototypeBean2);

        ac.close();
    }

    @Scope("prototype")
    static class PrototypeBean{
        @PostConstruct
        public void init(){
            System.out.println("PrototypeBean.init");
        }

        @PreDestroy
        public void destroy(){
            System.out.println("PrototypeBean.destroy");
        }
    }
}
```
- 스프링 컨테이너에서 빈을 조회할 때 생성되고 초기화 메서드 실행
- 프로토타입 빈을 2번 조회하여 다른 스프링 빈 생성
- 스프링 컨테이너가 생성, 의존관계 주입, 초기화까지만 관여하기 때문에 스프링 컨테이너 종료시 `@PreDestroy` 같은 종료 메서드 실행하지 않음 ➡️필요시 직접 호출해야 함






<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

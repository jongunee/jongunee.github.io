---
layout: post
title: IoC, DI, 컨테이너
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# IoC, DI, 컨테이너

* toc
{:toc .large-only}

## IoC(Inversion of Control): 제어의 역전

- 개발자가 원하는 데로 객체 생성 및 호출해서 개발자가 직접 제어
- 하지만 프레임워크가 제어권을 가져가게 되어 IoC라고 불린다. 

__프레임워크 vs 라이브러리__

- __프레임워크:__ 내가 작성한 코드를 제어하고 대신 실행
- __라이브러리:__ 내가 작성한 코드가 직접 제어의 흐름을 담당

## DI(Dependency Injection): 의존관계 주입

- `OrderServiceImple`은 `DiscountPolicy` 인터페이스에만 의존
- 어떤 구현 객체가 사용될지(FixDiscountPolicy일지 RateDiscountPolicy일지) 모름

__정적 클래스 의존관계__

- 정적 의존 관계는 클래스가 사용하는 import 코드만 보고 쉽게 판단 가능
![그림1](/assets/img/spring/order_domain_class.png)
- `OrderServiceImpl`은 `MemberRepository`, `DiscountPolicy`에 의존한다는 것을 알 수 있음
- 하지만 어떤 객체가 `OrderServiceImple`에 주입 될지는 알수 없음

__동적 클래스 의존 관계__

- 애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계

![그림2](/assets/img/spring/order_domain_object2.png)

- 실행 시점에 파악이 가능
- __의존관계 주입:__ 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성해서 실제 의존관계가 연결되는 것

## 컨테이너(IoC 컨테이너, DI 컨테이너)

>  AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 <span style='background-color: #f5f0ff'>IoC 컨테이너 또는 DI 컨테이너</span>라 한다.

- 의존관계 주입에 초점을 맞추기 떄문에 최근에는 주로 DI 컨테이너라고 함
- 어셈블러, 오브젝터 팩토리 등으로 불리기도 함


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

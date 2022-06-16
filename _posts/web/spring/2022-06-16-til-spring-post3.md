---
layout: post
title: 객체 지향 원리 적용
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 객체 지향 원리 적용

* toc
{:toc .large-only}


## 객체 지향 문제점

- 역할과 구현 분리 ➡️ OK
- 다형성 활용, 인터페이스 구현 객체 분리 ➡️ OK
- <mark>OCP, DIP 객체지향 설계 원칙 준수 ➡️ NO</mark>
    - __DIP:__ 추상 클래스(인터페이스) 뿐만 아니라 구현 클래스에도 의존
    - __OCP:__ FixDiscountPolicy에서 RateDiscountPolicy로 변경되면서 소스코드가 변경됨

__DIP 위반__
![그림1](/assets/img/spring/dependency_relationship.png)

__OCP 위반__
![그림2](/assets/img/spring/policy_changement.png)

__인터페이스에만 의존하도록 설계 변경__

![그림3](/assets/img/spring/order_dependency_target.png)


## 객체지향 문제점 해결

의존성 문제부터 해결

```java
//private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
private DiscountPolicy discountPolicy;
```
DIP는 지켰지만 구현체가 없기 떄문에 `NullPointerException` 발생

__관심사 분리__

구현 객체를 생성하고 연결을 책임지는 별도 설정 클래스 생성

__MemberServiceImpl__ - 생성자 주입

```java
private final MemberRepository memberRepository;

public MemberServiceImpl(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
}
```
- MemberServiceImple 코드 안에 MemoryMemberRepository 관련 코드는 없음  
➡️ MemoryMemberRepository에 의존하지 않고 인터페이스에만 의존
- 어떤 레포지토리를 사용할 것인지는 AppConfig가 결정

![그림4](/assets/img/spring/member_service_appConfig.png)
- AppConfig가 생성과 연결을 담당
- 인터페이스에만 의존

__OrderServiceImpl__ - 생성자 주입

```java
private final MemberRepository memberRepository;
private DiscountPolicy discountPolicy;


public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```
- OrderServiceImple 코드 안에 MemoryMemberRepository 관련 코드와 FixDiscountPolicy 관련 코드는 없음  
➡️ MemoryMemberRepository와 FixDiscountPolicy에 의존하지 않고 인터페이스에만 의존
- 어떤 레포지토리를 사용할 것인지, 어떤 할인 정책을 사용할 것인지는 AppConfig가 결정

__AppConfig__

```java
public class AppConfig {

    public MemberService MemberServiceImpl() {
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService() {
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
```
- AppConfig에서 실제 동작에 필요한 구현 객체 생성
    - MemberServiceImpl
    - MemoryMemberRepository
    - OrderServiceImpl
    - FixDiscountPolicy
- 외부(AppConfig)에서 `MemberServiceImpl`과 `OrderServiceImpl`에 의존관계를 주입하기 때문에 이를 DI(Dependency Injection), 의존관계 주입, 의존성 주입이라 한다.

__MemberApp__

```java
//MemberService memberService = new MemberServiceImpl();
AppConfig appConfig = new AppConfig();
MemberService memberService = appConfig.memberService();
```
AppConifg 적용

__OrderApp__

```java
AppConfig appConfig = new AppConfig();

//MemberService memberService = new MemberServiceImpl();
MemberService memberService = appConfig.memberService();

//OrderService orderService = new OrderServiceImpl();
OrderService orderService = appConfig.orderService();
```
AppConfig 적용

## 테스트 파일 수정

__MemerServiceTest__

```java
//MemberService memberService = new MemberServiceImpl();
MemberService memberService;

@BeforeEach
public void beforeEach(){
    AppConfig appConfig = new AppConfig();
    memberService = appConfig.memberService();
}
```
- __\@BeforEach__ 어노테이션은 테스트 실행하기 전에 해당 메서드를 실행하도록 한다
- AppConfig 적용

__OrderServiceTest__

```java
//MemberService memberService = new MemberServiceImpl();
//OrderService orderService = new OrderServiceImpl();

MemberService memberService;
OrderService orderService;

@BeforeEach
public void beforeEach(){
    AppConfig appConfig = new AppConfig();
    memberService = appConfig.memberService();
    orderService = appConfig.orderService();
}
```
AppConfig 적용

## AppConfig 리팩터링

역할에 따른 구현이 잘 안 보임

```java
public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy(){
        return new FixDiscountPolicy();
    }
}
```
- 메서드를 구현해서 중복되는 `new MemoryMemberRepository()` 제거
- 레포지토리나 할인 정책이 바뀌면 한 부분만 바꾸면 됨
- 역할 구현 부분이 한 눈에 보임 



<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

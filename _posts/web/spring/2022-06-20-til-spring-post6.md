---
layout: post
title: @Configuration
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# @Configuration

* toc
{:toc .large-only}

## @Configuration과 싱글톤

__AppConfig__

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }
}
```
- `memberService` 빈: `memberRepository()` 호출
    - `new MemoryMemberRepository()` 호출
- `orderService` 빈: `memberRepository()` 호출
    - `new MemoryMemberRepository()` 호출
- 2개의 `MemoryMemberRepository`가 생성되면 싱글톤이 깨질까?


__테스트 코드__

__검증용으로 임시 코드 추가__

```java
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository;
    //테스트 용도
    public MemberRepository getMemberRepository() {
        return memberRepository;
    }
}
public class OrderServiceImpl implements OrderService {
    private final MemberRepository memberRepository;
    //테스트 용도
    public MemberRepository getMemberRepository() {
        return memberRepository;
    }
}
```

__테스트 코드__

```java
public class ConfigurationSingletonTest {

    @Test
    void configurationTest(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository = " + memberRepository1);
        System.out.println("orderService -> memberRepository = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        Assertions.assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);
        Assertions.assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
    }
}
```

__AppConfig에 로그 추가__

```java
@Bean
public MemberService memberService() {
    System.out.println("Call AppConfig.memberService");
    return new MemberServiceImpl(memberRepository());
}

@Bean
public MemberRepository memberRepository() {
    System.out.println("Call AppConfig.memberRepository");
    return new MemoryMemberRepository();
}

@Bean
public OrderService orderService() {
    System.out.println("Call AppConfig.orderService");
    return new OrderServiceImpl(memberRepository(), discountPolicy());
}
```

__예상 결과__
- 총 3번
    - 스프링 컨테이너가 스프링 빈에 등록하기 위해 `memberRepository()` 호출
    - `memberService()` 로직에서 `memberRepository()` 호출
    - `orderService()` 로직에서 `memberRepository()` 호출 

__실제 결과__

- 1번 호출

## @Configuration 바이트코드 조작

스프링이 자바 코드를 조작할 수는 없음

__테스트 코드__

```java
@Test
void configurationDeep(){
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    AppConfig bean = ac.getBean(AppConfig.class);

    System.out.println("bean = " + bean.getClass());
}
```
- `AnnotationConfigApplicationContext`에 파라미터로 넘겨 `AppConfig`도 스프링 빈이 됨
- `AppConfig` 클래스 출력 결과: 
```java
bean = class hello.core.AppConfig$$EnhancerBySpringCGLIB$$993824f4
```
- 뒤에 이상한 값들이 붙음

![그림1](/assets/img/spring/configuration_cglib.png)

- 스프링이 `CGLIB`라는 바이트코드 조작 라이브러리를 사용해 `AppConfig` 클래스를 상속받은 임의의 다른 클래스를 만들어 스프링 빈에 등록한 것
- 이 임의의 다른 클래스가 싱글톤을 보장

__AppConfig@CGLIB 예상 코드__

```java
@Bean
public MemberRepository memberRepository() {
    
    if (memoryMemberRepository가 이미 스프링 컨테이너에 등록되어 있으면?) {
        return 스프링 컨테이너에서 찾아서 반환;
    } else { //스프링 컨테이너에 없으면
        기존 로직을 호출해서 MemoryMemberRepository를 생성하고 스프링 컨테이너에 등록
        return 반환
    }
}
```
- @Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환
- 없으면 생성해서 스프링 빈으로 등록하고 반환 코드가 동적으로 만들어짐
- `AppConfig@CGLIB`는 `AppConfig`의 자식 타입이기 때문에 `AppConfig` 타입으로 조회 가능하고 문제 없이 동작

__@Configuration을 제거한다면?__

- 순수 AppConfig로 스프링 빈 등록
```java
bean = class hello.core.AppConfig
```
- 싱글톤이 꺠짐
- 전에 예상했던대로 `memberRepository` 3번 출력
```java
call AppConfig.memberService
call AppConfig.memberRepository
call AppConfig.orderService
call AppConfig.memberRepository
call AppConfig.memberRepository
```
- <mark>`@Bean`만 사용해도 스프링 빈으로 등로되지만 싱글톤은 보장되지 않음</mark>

<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

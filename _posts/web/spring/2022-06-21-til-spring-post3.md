---
layout: post
title: 스프링 의존관계 주입 및 옵션 처리
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 의존관계 주입 및 옵션 처리

* toc
{:toc .large-only}

## 의존관계 주입 방법

- 생성자 주입
- 수정자 주입(setter 주입)
- 필드 주입
- 일반 메서드 주입

### 생정자 주입

> 생성자 주입이란 이름 그대로 생성자를 통해서 의존 관계를 주입하는 방법

- 생성자 호출시점에 딱 1번만 호출되는 것 보장
- 불변, 필수 의존 관계에 사용

__OrderServiceImpl 클래스__

```java
@Component
public class OrderServiceImpl implements OrderService{
    ...

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```
- 생성자 생성시 빈 자동 등록 및 주입됨
- 생성자가 1개라면 `@Autowired` 생략해도 스프링 빈 자동 주입

### 수정자 주입(setter 주입)

> 수정자 주입이란 필드 값을 변경하는 `setter` 수정자 메서드를 통해 의존관계 주입하는 방식

```java
@Component
public class OrderServiceImpl implements OrderService{

    private MemberRepository memberRepository;

    private DiscountPolicy discountPolicy;
    
    @Autowired
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
    ...
}
```
- 선택, 변경 가능성이 있는 의존관계에 사용
- 자바빈 프로퍼티 규약의 수정자 메서드 방식 사용
- 주입할 대상이 없다면 오류 발생
- 주입할 대상이 없더라도 동작하게 하려면 다음 코드로 지정
```java
@Autowired(required = false)
```

### 필드 주입

> 필드 주입이란 필드에 바로 주입하는 방식

```java
@Component
public class OrderServiceImpl implements OrderService{

    @Autowired
    private MemberRepository memberRepository;
    @Autowired
    private DiscountPolicy discountPolicy;
}
```

- 코드는 간결하지만 외부에서 변경이 불가능해 테스트하기가 어렵다
- DI 프레임워크가 없으면 아무것도 할 수 없음
- 순수 자바 테스트 코드에서는 `@Autowired` 동작하지 않음
- 스프링 설정을 목적으로 하는 `@Configuration` 같은 곳에서만 특별한 용도로 사용하기도 함
- 사용하지 말자!

### 일반 메서드 주입

> 일반 메서드 주입이란 일반 메서드를 통해 주입 받는 것

```java
@Component
public class OrderServiceImpl implements OrderService {
  private MemberRepository memberRepository;
  private DiscountPolicy discountPolicy;
  
  @Autowired
  public void init(MemberRepository memberRepository, DiscountPolicy
  discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }
}
```
- 한 번에 여러 필드 주입 가능
- 의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작

## 옵션 처리

__자동 주입 대상을 옵션으로 처리하는 방법__

```java
public class AutoWiredTest {

    @Test
    void AutowiredOption(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestBean.class);
    }

    static class TestBean{
        @Autowired(required = false)
        public void setNoBean1(Member noBean1){
            System.out.println("noBean1 = " + noBean1);
        }

        @Autowired
        public void setNoBean2(@Nullable Member noBean2){
            System.out.println("noBean2 = " + noBean2);
        }

        @Autowired
        public void setNoBean3(Optional<Member> noBean3){
            System.out.println("noBean3 = " + noBean3);
        }

    }
}
```

- `@Autowired(required = false)`: 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
- `org.springframework.lang.@Nullable`: 자동 주입할 대상이 없으면 `null` 입력
- `Optional<>`: 자동 주입할 대상이 없으면 `Optional.empty` 입력

## 왜 생성자 주입?

__불변__

- 대부분의 의존관계 주입은 한 번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없음
- 수정자 주입을 사용하면 `setXxx` 메서드를 `public`으로 열어두어야 함
  - 실수로 변경할 가능성 있음
  - 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방식이 아님
- 생성자 주입은 객체 생성시 1번만 호출되기 때문에 이후 호출되는 일이 없어 불변하게 설계 가능

__final 키워드__

```java
@Component
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;
  
  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy
  discountPolicy) {
    this.memberRepository = memberRepository;
  }
//...
}
```
- 이와 같이 생성자에서 값이 설정되지 않는 오류를 컴파일 시점에서 막아줌
- 생성자 주입을 제외한 나머지 주입 방식은 모두 생성자 이후에 호출되기 때문에 필드에 `final` 키워드 사용이 불가능










<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

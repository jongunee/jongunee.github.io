---
layout: post
title: \@Qualifier, @Primary, 애너테이션 생성
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# \@Qualifier, @Primary, 애너테이션 생성

* toc
{:toc .large-only}

## 조회할 빈이 2개 이상일 떄

- `@Autowired`는 타입으로 조회하기 때문에 다음 코드와 유사한 동작을 한다

```java
ac.getBean(DiscountPolicy.class)
```

- 아래처럼 해당 타입의 빈이 여러개라면 문제 발생
```java
@Component
public class FixDiscountPolicy implements DiscountPolicy {}
```
```java
@Component
public class RateDiscountPolicy implements DiscountPolicy {}
```
- 하위 타입으로 지정할 수도 있지만 이는 DIP 위배이며 유연성이 떨어짐

### @Autowired 필드명 매칭

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy rateDiscountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = rateDiscountPolicy;
}
```
또는

```java
@Autowired
private DiscountPolicy rateDiscountPolicy
```
- `@Autowired`는 타입 매칭을 시도했을 때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 추가 매칭
- 필드명을 빈 이름으로 변경해서 해결 가능
- 필드명 `rateDiscountPolicy`으로 정상 주입

### Qualifier

```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy{}
```
```java
@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy{}
```

- 빈 등록시 `@Qualifier`로 추가 구분자를 붙여준다 
- 추가 구분자일뿐 빈 이름을 변경하는 것은 아님

__생성자 자동 주입__

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```
주입시 등록한 구분자 이름을 적어줌

__수정자 자동 주입__

```java
@Autowired
public DiscountPolicy setDiscountPolicy(@Qualifier("mainDiscountPolicy")
DiscountPolicy discountPolicy) {
  this.discountPolicy = discountPolicy;
}
```
- 만약 못찾으면 `mainDiscountPolicy` 스프링 빈을 추가로 찾음
- 그래도 없으면 `NoSuchBeanDefinitionException` 예외 발생
- 모든 코드에 `@Qualifier`를 붙여주어야 하기 때문에 번거로움

### @Primary

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy{}
```

- `@Primary`는 우선순위를 정해주어 빈이 여러 개라면 `@Primary`를 가진 빈애 우선권을 가져간다
- `@Primary`보다는 `@Qualifier`가 우선권이 높다
- 스프링은 <span style='background-color: #f5f0ff'>자동보다는 수동</span>, <span style='background-color: #f5f0ff'>넓은 범위 보다는 좁은 범위</span>의 선택권이 우선 순위가 높다

## 애너테이션 직접 생성

중복 빈을 조회할 때 구분자를 지정했던 `@Qualifier` 방식을 새로 직접 생성

__정의__
```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
@Qualifier("mainDiscountPolicy")
public @interface MainDiscountPolicy {}
```

__빈 등록__

```java
@Component
@MainDiscountPolicy
public class RateDiscountPolicy implements DiscountPolicy {}
```

__생성자 또는 수정자__
```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @MainDiscountPolicy DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```
- 애너테이션에는 상속 개념이 없음
- 여러 애너테이션을 모아서 사용하는 기능은 스프링이 제공
- 다른 애너테이션들 조합해서 사용 가능





<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

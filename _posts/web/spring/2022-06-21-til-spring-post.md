---
layout: post
title: 컴포넌트 스캔
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 컴포넌트 스캔

* toc
{:toc .large-only}

## 컴포넌트 스캔 필요성

- 모든 메서드에 하나 하나 스프링 빈 등록을 위해 `@Bean`을 추가하는 것은 너무 귀찮은 일
- 스프링은 설정 정보가 없더라도 자동으로 스프링 빈을 등록하는 <span style='background-color: #f5f0ff'>컴포넌트 스캔</span> 기능 제공
- 의존관계 자동으로 주입하는 `@Autowired`도 제공

__AutoAppConfig__

```java
@Configuration
@ComponentScan(
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
)
public class AutoAppConfig {

}
```
- `@Configuration`이 붙은 `AppConfig`, `TestConfig` 등 설정 정보를 제외

__MemberService 관련 @Component 추가__

```java
@Component
public class MemberServiceImpl implements MemberService{
    ...
    @Autowired
    public MemberServiceImpl(MemberRepository memberRepository) {}
}
```
- 여기서 `@Autowired` 기능은 다음 코드와 같은 기능
```java
ac.getBean(MemberReporitory.class);
```
- 생성자에서 여러 의존관계도 한 번에 주입

__OrderService 관련 @Component 추가__

```java
@Component
public class OrderServiceImpl implements OrderService{
    ...
    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {}
}

@Component
public class RateDiscountPolicy implements DiscountPolicy {}
```

```java
public class AutoAppConfigTest {
    @Test
    void basicScan(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);

        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```

## @ComponentScan

![그림1](/assets/img/spring/component.png)

- `@ComponentScan`은 `@Component`가 붙은 모든 클래스를 스프링 빈으로 등록
- 스프링 빈 기본 이름: 클래스명을 사용하되 맨 앞글자만 소문자
  - ex) MemberServiceImpl 클래스 ➡️memberServiceImpl
  - 직접 지정: `@Component("memberServiceImpl2")`

## @Autowired

![그림2](/assets/img/spring/autowired_di1.png)

- `@Autowired` 지정시 스프링 컨테이너가 자동으로 스프링 빈 찾아 주입
- 조회시 기본적으로 타입이 같은 빈을 찾아 주입

![그림3](/assets/img/spring/autowired_di2.png)

- 생성자에 파라미터가 여러개라도 모두 자동으로 찾아 주입

## 탐색 위치

```java
@ComponentScan(
        basePackages = "hello.core", "hello.service"
)
```
- 모든 자바 클래스를 컴포넌트 스캔하려면 시간이 오래 걸리기 때문에 필요한 위치부터 탐색하도록 지정
- `basePackages`: 탐색할 패키지 시작 위치로 지정하여 하위 패키지 모두 탐색
- 여러 패키지 지정도 가능
- `basePackageClasses`: 지정한 클래스의 패키지를 탐색 시작 위치로 지정
- 지정하지 않으면 `@ComponentScan`이 붙은 클래스의 패키지가 시작 위치로 지정

__권장 방법__

- 시작 패키지 위치를 지정하지 않고
- 설정 정보 클래스 위치를 프로젝트 최상단에 두는 것
- 그렇다면 모든 하위 클래스가 컴포넌트 스캔 대상이 됨
- 관례적으로 스프링 부트의 대표 시작 정보인 `@SpringBootApplication`을 이 프로젝트 시작 루트 위치에 둔다 - `@ComponentScan` 포함하고 있음

## 컴포넌트 스캔 기본 대상

다음 항목들 모두 내부적으로 `@Component`를 포함하고 있기 때문에 모두 스캔 대상
- `@Component`:
  - 컴포넌트 스캔에서 사용
- `@Controller`:
  - 스프링 MVC 컨트롤러에서 사용
  - 스프링 MVC 컨트롤러로 인식
- `@Service`:
  - 스프링 비즈니스 로직에서 사용
  - 특별한 처리는 없음
  - 개발자가 비즈니스 로직이 어디에 있는지 계층을 인식하는 데 도움
- `@Repository`:
  - 스프링 데이터 접근 계층에서 사용
  - 스프링 데이터 접근 계층으로 인식
  - 데이터 계층의 예외를 스프링 예외로 변환
- `@Configuration`:
  - 스프링 설정 정보에서 사용
  - 스프링 설정 정보 인식
  - 스프링 빈이 싱글톤을 유지하도록 추가처리

애너테이션 속 특정 애너테이션을 인식하는 것은 자바가 아니라 스프링의 기능이며 스프링이 애너테이션에 따라 부가 기능도 수행


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

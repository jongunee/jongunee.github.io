---
layout: post
title: 컴포넌트 필터, 중복 등록과 충돌
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 컴포넌트 필터, 중복 등록과 충돌

* toc
{:toc .large-only}

## 필터 예제

- `includeFilters`: 컴포넌트 스캔 대상 추가 지정
- `excludeFilters`: 컴포넌트 스캔에서 제외한 대상 지정

__컴포넌트 스캔 대상 추가 애너테이션__

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyIncludeComponent {

}
```

__컴포넌트 스캔 예외 대너테이션__

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyExcludeComponent {

}
```

__컴포넌트 스캔 대상 추가 클래스__

```java
@MyIncludeComponent
@Component
public class BeanA {
}
```

__컴포넌트 스캔 대상 제외 클래스__

```java
@MyExcludeComponent
@Component
public class BeanB {
}
```

__테스트 코드__

```java
public class ComponentFilterAppConfigTest {
    @Test
    void filterScan(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(ComponentFilterAppConfig.class);
        BeanA beanA = ac.getBean("beanA", BeanA.class);
        Assertions.assertThat(beanA).isNotNull();

        assertThrows(
                NoSuchBeanDefinitionException.class,
                () -> ac.getBean("beanB", BeanB.class));
    }

    @Configuration
    @ComponentScan(
            includeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
            excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
    )
    static class ComponentFilterAppConfig{

    }
}
```
- `includeFilters`에 `@MyIncludeComponent` 애너테이션을 추가해 `BeanA`가 스프링 빈에 등록됨
- `excludeFilters`에 `@MyExcludeComponent` 애너테이션을 추가해 `BeanB`가 스프링 빈에 등록되지 않음

## FilterType 옵션

- `ANNOTATION`: 기본값, 애너테이션 인식 및 동작  
  ex) `org.example.SomeAnnotation`
- `ASSIGNABLE_TYPE`: 지정한 타입과 자식 타입 인식 및 동작
  ex) `org.example.SomeClass`
- `ASPECTJ`: AspectJ 패턴 사용  
  ex) `org.example..*Service+`
- `REGEX`: 정규 표현식  
  ex) `org\.example\.Default.*`
- `CUSTOM`: `TypeFilter`라는 인터페이스 구현 및 처리  
  ex) `org.example.MyTypeFilter`

## 중복 등록과 충돌

컴포넌트 스캔에서 같은 빈 이름을 등록해서 충돌이 나느 상황이 발생할 수 있다.

1. 자동 빈 등록 VS 자동 빈 등록

- `ConflictingBeanDefinitionException` 예외 발생

2. 수동 빈 등록 VS 자동 빈 등록

__예제 코드__

```java
public class AutoAppConfig {
    @Bean(name = "memoryMemberRepository")
    MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}
```
AutoAppConfig에서 같은 이름으로 빈을 추가

__결과__

- PASS - 문제 없음
- 수동으로 등록한 빈이 우선권을 갖기 때문에 오버라이딩 된다
- 하지만 이것을 의도한 것이 아니라면 잡기 어려운 에러로 이어질 수 있음

## 수동 빈 중복 등록 에러 처리

`resoures\application.properties`에 설정 가능
```java
spring.main.allow-bean-definition-overriding=true //수동 빈 오버라이딩 허용
spring.main.allow-bean-definition-overriding=false //수동 빈 오버라이딩 막음
```











<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

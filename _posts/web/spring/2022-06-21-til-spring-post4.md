---
layout: post
title: 롬복
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 롬복

* toc
{:toc .large-only}

## 롬복 적용

__build.gradle__

```java
//lombok 설정 추가 시작
configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

dependencies{
  ...
  compileOnly 'org.projectlombok:lombok'
  annotationProcessor 'org.projectlombok:lombok'
  testCompileOnly 'org.projectlombok:lombok'
  testAnnotationProcessor 'org.projectlombok:lombok'
}
```
- 스프링 부트에서 처음 생성할 때 dependency에서 lombok 추가해도 됨


__롬복 애너테이션__ 

- `@Getter`: `getXxx` 메서드 자동 구현
- `@Setter`: `setXxx` 메서드 자동 구현
- `@ToString` `toString` 메서드 자동 구현
- ` `@RequiredArgsConstructor`: 클래스 내에 `final` 키워드를 가진 변수를 이용해 생성자 자동 생성 ➡️File structure에서 확인 가능

__롬복 적용 전__

```java
@Component
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;
  
  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy
  discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }
}
```

__롬복 적용 후__

```java
@Component
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;
}
```

<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

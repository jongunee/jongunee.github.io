---
layout: post
title: List, Map, 자동/수동 실무 운영 기준
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# List, Map, 자동/수동 실무 운영 기준

* toc
{:toc .large-only}

## 조회한 빈이 모두 필요할 때

의도적으로 해당 타입의 스프링 빈이 모두 필요한 경우가 있다.  
ex) 할인 서비스를 제공하는데, 클라이언트가 할인 종류를 선택할 수 있는 경우

__테스트 코드__

```java
public class AllBeanTest {
    @Test
    void findAllBean() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class);

        DiscountService discountService = ac.getBean(DiscountService.class);
        Member member = new Member(1L, "userA", Grade.VIP);
        int discountPrice = discountService.discount(member, 10000, "fixDiscountPolicy");

        assertThat(discountService).isInstanceOf(DiscountService.class);
        assertThat(discountPrice).isEqualTo(1000);

        int rateDiscountPrice = discountService.discount(member, 20000, "rateDiscountPolicy");
        assertThat(rateDiscountPrice).isEqualTo(2000);
    }
    static class DiscountService {
        private final Map<String, DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policies;

        @Autowired
        public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
            this.policyMap = policyMap;
            this.policies = policies;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policies = " + policies);
        }

        public int discount(Member member, int price, String discountCode) {
            DiscountPolicy discountPolicy = policyMap.get(discountCode);
            return discountPolicy.discount(member, price);
        }
    }
}
```

__로직__

- `DiscountService`는 `Map`으로 모든 `DiscountPolicy`를 주입받음
  - `fixDiscountPolicy`
  - `rateDiscountPolicy`
- `discount()`메서드는 `discountCode`로 들어온 `fixDiscountPolicy` 또는 `rateDiscountPolicy` 스프링 빈을 찾아 실행


__주입__

- `Map<String, DiscountPolicy>`:
  - Key: 스프링 빈 이름
  - Value: 해당 스프링 빈 이름의 `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담음
- `List<DiscountPolicy>`:
  - `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담음
- 해당 타입 없으면 빈 컬렉션이나 `Map` 주입


## 자동, 수동 실무 운영 기준

### 자동 빈 등록

- 점점 더 자동화를 선호하는 추세
- 스프링 부트는 컴포넌트 스캔을 기본, 스프링 부트의 다양한 스프링 빈들도 조건 맞으면 자동 등록
- 자동 빈 등록을 사용해도 OCP, DIP 지킬 수 있음

### 수동 빈 등록

- 업무 로직 빈:
  - 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 리포지토리 등에 사용
  - 문제가 어디서 발생했는지 파악하기 쉬움
  - 숫자도 많고 유사 패턴이 있음
  - 자동 기능 권장
- 기술 지원 빈:
  - 기술적 문제나 공통 관심사(AOP) 처리 시 사용
  - 문제가 어디서 발생했는지 파악하기 어려움
  - 업무 로직에 비해 수가 적고 애플리케이션 전반에 걸쳐 광범위하게 영향을 미침
  - 수동 빈 등록 권장

하지만 비즈니스 로직이라도 수동 빈 등록이 좋을 때도 있다. 위에 `DiscountService` 의존 관계 자동 주입으로 `Map<String, DiscountPolicy>`에 주입을 받는 상황이라면 코드만 보고 한번에 파악하기 어려움

```java
@Configuration
public class DiscountPolicyConfig {
  @Bean
  public DiscountPolicy rateDiscountPolicy() {
    return new RateDiscountPolicy();
  }

  @Bean
  public DiscountPolicy fixDiscountPolicy() {
    return new FixDiscountPolicy();
  }
}
```
- 이처럼 설정 정보로 만들고 수동 등록하면 한 번에 보기 쉬움
- 또는 자동으로 한다면 특정 패키지에 같이 묶어놓는 것이 좋음


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

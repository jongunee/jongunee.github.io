---
layout: post
title: 회원 주문 예제 - 새로운 정책 구현
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 회원 주문 예제 - 새로운 정책 구현

* toc
{:toc .large-only}

## 새로운 할인 정책 개발

기존 고정 할인 정책에서 정률 할인 정책으로 변경

__RateDiscountPolicy 추가__

![그림1](/assets/img/spring/discount_policy_diagram.png)

__RateDiscountPolicy 코드__

```java
public class RateDiscountPolicy implements DiscountPolicy{

    private int discountPercent = 10;
    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return price * discountPercent / 100;
        }
        else{
            return 0;
        }
    }
}
```

## 할인 정책 테스트

- VIP 등급 테스트
```java
class RateDiscountPolicyTest {
    RateDiscountPolicy discountPolicy = new RateDiscountPolicy();

    @Test
    @DisplayName("VIP는 10% 할인이 적용되어야 한다")
    void vip_O() {
        //given
        Member member = new Member(1L, "memberVIP", Grade.VIP);
        //when
        int discount = discountPolicy.discount(member, 10000);
        //then
        Assertions.assertThat(discount).isEqualTo(1000);
    }
}
```

- Basic 등급 테스트
```java
class RateDiscountPolicyTest {
 @Test
    @DisplayName("VIP가 아니면 할인이 적용되지 않아야 한다")
    void vip_x(){
        //given
        Member member = new Member(2L, "memberBasic", Grade.BASIC);
        //when
        int discount = discountPolicy.discount(member, 10000);
        //then
        Assertions.assertThat(discount).isEqualTo(0);
    }
}
```
- 테스트를 할 떄 오류 테스트도 해봐야 한다.
- 예를들면 Basic 등급 할인 금액을 1000원으로 해서 테스트를 했을 때 FAIL이 되는지 확인 해봐야 한다.

__import static__

```java
import static org.assertj.core.api.Assertions.assertThat;

assertThat(discount).isEqualTo(0);
```
- 정적 메소드를 `import static` 을 사용해서 `import` 하면 클래스명 없이 메서드 사용 가능  
ex) Assertions.assertThat() ➡️ assertThat()
- 하지만 같은 클래스 내에 동일한 이름의 메소드가 있으면 클래스 자신의 메서드가 우선

## 할인 정책 적용

__OrderServiceImpl__

```java
//private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
```
잘 작동한다


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

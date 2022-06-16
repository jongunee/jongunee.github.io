---
layout: post
title: 스프링 예제 - 주문 할인 도메인
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 예제 - 주문 할인 도메인

* toc
{:toc .large-only}

## 비즈니스 요구사항 설계

- 주문과 할인 정책
  - 회원은 상품 주문 가능
  - 회원 등급에 따른 할인 정책 적용
  - __할인 정책:__ 모든 VIP에 1000원 고정 금액 할인
  - 할인 정책은 변경 가능성이 높으며 아직 미정

## 주문 할인 도메인 설계

__주문 도메인 관계 및 역할__

![그림1](/assets/img/spring/order_domain.png)

1. __주문 생성:__  
  클라이언트가 주문 생성 요청

2. __회원 조회:__  
  주문 서비스가 할인을 위해 회원 저장소에서 회원 조회

3. __할인 적용:__  
  주문 서비스가 회원 등급에 따른 할인 여부 할인 정책에 위임

4. __주문 결과 반환:__  
  주문 서비스가 할인 결과를 포함한 주문 결과 반환

역할과 구현을 분리해서 유연하게 변경할 수 있도록 한다. 실제로는 주문 데이터를 DB에 저장하지만 생략하고 결과만 반환

__주문 도메인 클래스 다이어그램__

![그림2](/assets/img/spring/order_domain_class.png)

![그림3](/assets/img/spring/order_domain_object1.png)

![그림4](/assets/img/spring/order_domain_object2.png)

회원을 메모리가 아닌 DB에서 조회하고 정률 할인 정책으로 바뀌더라도 주문 서비스를 변경할 필요가 없다

## 할인 도메인 개발

__할인 정책 인터페이스__

```java
public interface DiscountPolicy {
    //@return 할인 대상 금액
    int discount(Member member, int price);
}
```

__정액 할인 정책 구현체__

```java
public class FixDiscountPolicy implements DiscountPolicy{

    private int discountFixAmount = 1000;

    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return discountFixAmount;
        }
        else{
            return 0;
        }
    }
}
```

- VIP만 할인

## 주문 도메인 개발

__주문 엔티티__

```java
public class Order {
    private Long memberId;  //long은 null값을 넣을 수 없음
    private String itemName;
    private int itemPrice;
    private int discountPrice;

    public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
        this.memberId = memberId;
        this.itemName = itemName;
        this.itemPrice = itemPrice;
        this.discountPrice = discountPrice;
    }

    public int calculatePrice(){
        return itemPrice - discountPrice;
    }

    public Long getMemberId() {
        return memberId;
    }

    public void setMemberId(Long memberId) {
        this.memberId = memberId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public int getItemPrice() {
        return itemPrice;
    }

    public void setItemPrice(int itemPrice) {
        this.itemPrice = itemPrice;
    }

    public int getDiscountPrice() {
        return discountPrice;
    }

    public void setDiscountPrice(int discountPrice) {
        this.discountPrice = discountPrice;
    }

    @Override
    public String toString() {
        return "Order{" +
                "memberId=" + memberId +
                ", itemName='" + itemName + '\'' +
                ", itemPrice=" + itemPrice +
                ", discountPrice=" + discountPrice +
                '}';
    }
}
```
- 주문에 필요한 멤버ID, 품명, 가격, 할인 가격 선언
- 생성자, getter 및 setter 

__주문 서비스 인터페이스__

```java
public interface OrderService {
    Order createOrder(Long memberId, String itemName, int itemPrice);
}
```

__주문 서비스 구현__

```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

__실행 - OrderAPp__

```java
public class OrderApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberServiceImpl();
        OrderService orderService = new OrderServiceImpl();

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);

        System.out.println("order = " + order);
        System.out.println("order.calculatePrice = " + order.calculatePrice());
    }
}
```

## 주문 할인 정책 테스트

```java
public class OrderServiceTest {
    MemberService memberService = new MemberServiceImpl();
    OrderService orderService = new OrderServiceImpl();

    @Test
    void createOrder(){
        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);

    }
}
```





<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

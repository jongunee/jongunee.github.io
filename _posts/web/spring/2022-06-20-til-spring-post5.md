---
layout: post
title: 싱글톤 컨테이너
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 싱글톤 컨테이너

* toc
{:toc .large-only}

## 웹 애플리케이션과 싱글톤

![그림1](/assets/img/spring/web_application_request.png)

- 스프링은 기업용 온라인 서비스 기술을 지원하기 위해 탄생
- 대부분 웹 애플리케이션 개발
- 웹에서 여러 고객이 동시에 요청

## 순수 DI 컨테이너

__테스트 소스코드__
```java
public class SingletonTest {

    @Test
    @DisplayName("스프링 없는 순수 DI 컨테이너")
    void pureContainer(){
        AppConfig appConfig = new AppConfig();
        //1. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService1 = appConfig.memberService();

        //2. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService2 = appConfig.memberService();

        //참조값이 다른 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        //memberService1 != memberService2
        Assertions.assertThat(memberService1).isNotSameAs(memberService2);
    }
}
```
- 보이는 것 처럼 순수 DI 컨테이너는 요청할 때마다 객체를 새로 생성
- 고객 트래픽이 초당 100이 나오면 초당 100개 이상의 객체가 생성 후 소멸
- 해결책: <span style='background-color: #f5f0ff'>싱글톤 패턴</span> - 객체가 딱 1개만 생성되고 공유하도록 설계 

## 싱글톤 패턴

> 싱글톤 패턴이란 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴. `private` 생성자를 사용해서 외부에서 임의로 `new` 키워드를 사용하지 못하도록 막는다.

__소스코드__

```java
public class SingletonService {

    private static final SingletonService instance = new SingletonService();

    public static SingletonService getInstance(){
        return instance;
    }

    private SingletonService(){
    }

    public void logic(){
        System.out.println("싱글톤 객체 로직 호출");    
    }
}
```
1. `static` 영역에 객체를 1개만 생성
2. 인스턴스를 조회할 수 있는 `public static` 메서드를 생성
    - 오직 이 `static` 메서드를 통해서만 조회가 가능
3. `private`생성자를 선언해서 외부에서 `new` 키워드를 사용해 객채를 생성하는 것을 막음

__싱글톤 패턴을 적용한 객체 사용__

```java
@Test
@DisplayName("싱글톤 패턴을 적용한 객체 사용")
void singletonServiceTest(){
    SingletonService singletonService1 = SingletonService.getInstance();
    SingletonService singletonService2 = SingletonService.getInstance();

    System.out.println("singletonService1 = " + singletonService1);
    System.out.println("singletonService2 = " + singletonService2);

    assertThat(singletonService1).isSameAs(singletonService2);
}
```
- 미리 생성된 하나의 객체를 반환해주기 때문에 참조값이 같은 것 확인

## 싱글톤 패턴 문제점
 
- 구현 코드가 많이 들어감
- 클라이언트가 구체 클레스에 의존 ➡️ DIP 위반
- 클라이언트가 구체 클래승 의존해서 OCP 원칙 위반 가능성 높은
- 테스트하기 어려움
- 내부 속성 변경 및 초기화 어려움
- `private` 생성자로 자식 클래스 만들기 어려움
- 유연성 떨어짐
- 안티패턴이라 불리기도 함

## 싱글톤 컨테이너

- 싱글톤 패턴의 문제점 해결하며 객체 인스턴스를 싱글톤으로 관리
- 스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도 객체 인스턴스를 싱글톤으로 관리
- 스프링 컨테이너는 싱글톤 컨테이너역할을 하고 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라고 함
- 싱글톤 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지
    - 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 됨
    - DIP, OCP, 테스트, privste 생성자로부터 자유롭게 싱글톤 사용 가능

__스프링 컨테이너 테스트 코드__

```java
@Test
@DisplayName("스프링 컨테이너와 싱글톤")
void springContainer(){
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    //1. 조회: 호출할 때 마다 객체를 생성
    MemberService memberService1 = ac.getBean("memberService", MemberService.class);

    //2. 조회: 호출할 때 마다 객체를 생성
    MemberService memberService2 = ac.getBean("memberService", MemberService.class);

    //참조값이 다른 것을 확인
    System.out.println("memberService1 = " + memberService1);
    System.out.println("memberService2 = " + memberService2);

    //memberService1 != memberService2
    assertThat(memberService1).isSameAs(memberService2);
}
```

![그림2](/assets/img/spring/singletone_request.png)

- 스프링 컨테이너 적용
- 고객의 요청이 올 때마다 객체를 생서하는 것이 아니라 이미 만들어진 객체를 공유
- 효율적 재사용
- 스프링 기본 빈 등록 방식은 싱글톤
    - 새로운 객체를 생성해서 반환하는 기능도 있음

## 싱글톤 방식 주의점

- 여러 클라이언트가 하나의 가은 객체 인스턴스 공유
- 상태를 유지(stateful)하도록 설계하는 것이 아니라 무상태(stateless)로 설계해야 함
    - 특정 클라이언트에 의존적인 필드가 이쓰면안됨
    - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안됨
    - 가급적 읽기만 가능하도록 구현
    - 필드 대신에 자바에서 공유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야 함

__문제점 예시__

__StatefulService__

```java
public class StatefulService {

    private int price;

    public void order(String name, int price){
        System.out.println("name = " + name + " price = " + price);
        this.price = price;
    }

    public int getPrice(){
        return price;
    }
}
```

__StatefulServiceTest__

```java
class StatefulServiceTest {

    @Test
    void statefulServiceSingleton(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
        StatefulService statefulService1 = ac.getBean(StatefulService.class);
        StatefulService statefulService2 = ac.getBean(StatefulService.class);

        //ThreadA: A사용자 10000원 주문
        statefulService1.order("userA", 10000);

        //ThreadB: B사용자 20000원 주문
        statefulService2.order("userB", 20000);
        
        //ThreadA: 사용자A 주문 금액 조회
        int price = statefulService1.getPrice();
        System.out.println("price = " + price);

        Assertions.assertThat(statefulService1.getPrice()).isEqualTo(20000);
    }

    static class TestConfig{
        @Bean
        public StatefulService statefulService(){
            return new StatefulService();
        }
    }
}
```
- `StatefulService`에서 `price` 필드가 공유됨
- A가 코드를 호출하고 값을 조회하는 사이에 B가 코드를 호출했기 때문에 그 사이에 값이 변경됨
- 스프링 빈은 무상태로 설계되어야 함




<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

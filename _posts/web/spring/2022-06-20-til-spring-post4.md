---
layout: post
title: XML 설정, 빈 설정 메타 정보
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# XML 설정, 빈 설정 메타 정보

* toc
{:toc .large-only}

## 스프링 컨테이너 설정

![그림1](/assets/img/spring/xml_setting.png)
- 스프링 컨테이너는 유연하게 설게되어 있음
- 다양한 형식 설정 정보를 받아드릴 수 있음

__애너테이션 기반 자바 코드 설정__

- `new AnnotationConfigApplicationContext` 클래스를 사용해서 자바 코드로 된 설정 정보 넘김

__XML 설정__
- 최근 스프링 부트 사용으로 XMl 기반 설정은 잘 사용하지 않음
- 레거시 프로젝트들은 XML로 되어 있는 경우가 있음
- XML은 컴파일 없이 빈 설정 정보 변경 가능
- `GenericXmlApplicationContext`를 사용해서 xml 설정 파일을 넘김

## xml 코드 변환

__XmlAppContext__

```java
public class XmlApppContext {

    @Test
    void xmlAppContext(){
        ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```

__AppConfig.xml__

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="memberService" class="hello.core.member.MemberServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository" />
    </bean>

    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>

    <bean id="orderService" class="hello.core.order.OrderServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository"/>
        <constructor-arg name="discountPolicy" ref="discountPolicy"/>

    </bean>

    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy"/>
</beans>
```
- xml 포맷으로만 바꼈지 구성은 동일


## 스프링 빈 설정 메타 정보 - BeanDefinition

![그림2](/assets/img/spring/bean_definition.png)

- `BeanDifinition`이라는 추상화가 있기 때문에 스프링은 다양한 설정 형식 지원이 가능
- <span style='background-color: #f5f0ff'>역할과 구현</span>을 개념적으로 나눔
  - `xml`을 읽어 `BeanDifinition` 생성
  - 자바 코드를 읽어 `BeanDifinition` 생성
  - 스프링 컨테이너는 `BeanDifinition`만 알면 됨
- `BeanDifinition`을 빈 설정 메타정보
  - `@Bean` 또는 `<bean>` 당 각각 메타 정보 생성
스프링 컨테이너는 이 메타 정보를 기반으로 스프링 빈 생성

__BeanDefinition 상세__

![그림3](/assets/img/spring/bean_definition_detail.png)

- `AnnotationConfigApplicationContext`는 `AnnotationBeanDifinitionReader`를 사용해 `AppConfig.class`를 읽어 `BeanDifinition` 생성
- `GenericXmlApplicationContext`는 `xmlBeanDefinitionReader`는 `appConfig.xml`을 읽어 `BeanDifinition` 생성
- 새로운 형식 설정 정보가 추가되면 또다른 형식의 `BeanDefinitionReader`를 이용해 정보를 읽고 `BeanDifinition`을 생성해주면 됨

## BeanDefinition 정보

- `BeanClassName`: 생성할 빈의 클래스 명
- `factoryBeanName`: 팩토리 역할의 빈을 사용할 경우 이름  
ex) appConfig
- `factoryMethodName`: 빈을 생성할 팩토리 메서드 지정  
ex) memberService
- `Scope`: 싱글톤(기본값)
- `lazyInit`: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연처리 하는지 여부
- `InitMethodName`: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- `DestroyMethodName`: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- `Constructor arguments`, `Properties`: 의존관계 주입에서 사용

__확인 코드__

```java
public class BeanDifinitionTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("빈 설정 메타정보 확인")
    void findApplicationBean(){
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames){
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION){
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                " beanDefinition = " + beanDefinition);
            }
        }
    }
}
```

- `BeanDefinition`을 직접 생성해서 스프링 컨테이너에 등록도 가능하지만 실무에서 사용할 일은 거의 없음






<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

---
layout: post
title: XML 설정
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# XML 설정

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


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

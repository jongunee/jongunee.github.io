---
layout: post
title: 스프링 빈 조회
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 빈 조회

* toc
{:toc .large-only}

## 컨테이너에 등록 된 스프링 빈 조회

```java
public class ApplicationContextInfoTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("모든 빈 출력")
    void findBean(){
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("Name = " + beanDefinitionName + " object = " + bean);
        }
    }
}
```
- 스프링에 등록된 모든 빈 정보 출력
- `ac.getBeanDefinitionNames()`: 스프링에 등록된 모든 빈 이름 조회
- `ac.getBean(beanDefinitionName)`: 빈 이름으로 빈 객체 조회
- 스프링이 내부에서 사용하는 빈도 같이 출력

```java
@Test
@DisplayName("애플리케이션 빈 출력") //사용자가 지정한 빈만 출력 
void findApplicationBean(){
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();
    for (String beanDefinitionName : beanDefinitionNames) {
        BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

        if(beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION){
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("Name = " + beanDefinitionName + " object = " + bean);
        }
    }
}
```
- 사용자가 등록한 빈만 출력 가능
- `beanDefinition.getRole()`: 빈 구분 가능
    - `ROLE_APPLICATION`: 사용자가 등록한 빈
    - `ROLE_INFRASTRUCTURE`: 스프링 내부에서 사용하는 빈

## 스프링 빈 조회 테스트

```java
public class ApplicationContextBasicFindTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("빈 이름으로 조회")
    void findBeanByName(){
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }
```
- `ac.getBean(빈이름, 타입)`으로 스프링 빈 조회
```java
@Test
@DisplayName("이름 없이 타입으로만조회")
void findBeanByType(){
    MemberService memberService = ac.getBean(MemberService.class);
    assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
}
```
- `ac.getBean(타입)`으로 빈 이름 없이 스프링 빈 조회
```java
@Test
@DisplayName("구체 타입으로 조회")
void findBeanByName2(){
    MemberService memberService = ac.getBean("memberService", MemberServiceImpl.class);
    assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
}
```
- `ac.getBean(이름, 타입)`으로 조회할 때 인터페이스가 아닌 구체 타입으로도 조회 가능
- 하지만 유연성이 떨어지고 역할과 구현 관계가 모호해짐

```java
@Test
@DisplayName("빈 이름으로 조회X")
void findBeanByNameX(){
    assertThrows(NoSuchBeanDefinitionException.class,
            () -> ac.getBean("xxxxx", MemberService.class));
}
```
- `NoSuchBeanDefinitionException`: 빈 이름으로 조회했을 때 대상 빈이 없으면 에외 발생
- `assertThrows`: 해당 예외가 발생하는지 확인

## 스프링 빈 조회 - 동일한 타입이 둘 이상일 때

__타입 조회시 중복 오류 확인 테스트__

```java
public class ApplicationContextSameBeanFindTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);

    @Test
    @DisplayName("타입으로 조회시 같은 타입이 둘 이상 있으면 중복 오류가 발생한다")
    void findBeanTypeDuplicate(){
        assertThrows(NoUniqueBeanDefinitionException.class, () -> ac.getBean(MemberRepository.class));
    }

    @Configuration
    static class SameBeanConfig{
        @Bean
        public MemberRepository memberRepository1(){
            return new MemoryMemberRepository();
        }
        @Bean
        public MemberRepository memberRepository2(){
            return new MemoryMemberRepository();
        }
    }
}
```
- `NoUniqueBeanDefinitionException`: 이름 없이 타입으로 조회할 때 같은 타입의 스프링 빈이 둘 이상이면 오류 발생
- `assertThrows`로 해당 예외 발생하는지 확인

__스프링 빈 조회 테스트 - 이름 지정__

```java
@Test
@DisplayName("타입으로 조회시 같은 타입이 둘 이상 있으면, 빈 이름을 지정하면 된다")
void findBeanByName(){
    MemberRepository memberRepository = ac.getBean("memberRepository1", MemberRepository.class);
    assertThat(memberRepository).isInstanceOf(MemberRepository.class);
}
```
- 타입만 입력하면 오류가 발생
- 명확히 하기 위해 타입과 이름 같이 지정

__스프링 빈 특정 타입 모두 조회 테스트__

```java
@Test
@DisplayName("특정 타입 모두 조회하기")
void findAllBeanByType(){
    Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
    for(String key : beansOfType.keySet()){
        System.out.println("key = " + key + " value = " + beansOfType.get(key));
    }
    System.out.println("beansOfType = " + beansOfType);
    assertThat(beansOfType.size()).isEqualTo(2);
}
```
- `getBeansofTyp()`으로 해당 타입 모든 빈 조회 가능
- 스프링 빈 저장소에 Key - value 관계로 저장되는 것을 알 수 있음

## 스프링 빈 조회 - 상속 관계

![그림1](/assets/img/spring/spring_bean_inheritance.png)

- 부모 타입 조회시 자식 타입 함께 조회
- 모든 자바 객체의 최고 부모인 `Object` 타입 조회시 모든 스프링 빈 조회

__부모 타입 조회시 중복 오류 확인 테스트__

```java
public class ApplicationContextExtendsFindTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);

    @Test
    @DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 중복 오류가 발생한다")
    void findBeanByParentTypeDuplicate(){
        assertThrows(NoUniqueBeanDefinitionException.class,
                () -> ac.getBean(DiscountPolicy.class));
    }

    @Configuration
    static class TestConfig {
        @Bean
        public DiscountPolicy rateDiscountPolicy() {
            return new RateDiscountPolicy();
        }

        @Bean
        public DiscountPolicy fixDiscountPolicy() {
            return new FixDiscountPolicy();
        }
    }
}
```
- `NoUniqueBeanDefinitionException`: DiscountPolicy 조회시 자식 클래스가 2개 있기 때문에 중복 오류 발생
  - RateDiscountPolicy()
  - FixDiscountPolicy()
- `assertThrows`로 해당 예외 발생하는지 확인

__부모 타입 조회 테스트 - 이름 지정__

```java
@Test
@DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 빈 이름을 지정하면 된다")
void findBeanByParentTypeBeanName(){
    DiscountPolicy rateDiscountPolicy = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
    assertThat(rateDiscountPolicy).isInstanceOf(RateDiscountPolicy.class);
}
```
- 타입만 입력하면 오류가 발생
- 명확히 하기 위해 타입과 이름 같이 지정

__특정 하위 타입 조회 테스트__

```java
@Test
@DisplayName("특정 하위 타입으로 조회")
void findBeanBySubType(){
    RateDiscountPolicy bean = ac.getBean(RateDiscountPolicy.class);
    assertThat(bean).isInstanceOf(RateDiscountPolicy.class);
}
```
- 특정 하위 타입은 하나만 존재하기 때문에 오류가 나지 않음
- 좋은 방법은 아님

__부모 타입으로 모두 조회 테스트__

```java
@Test
@DisplayName("부모 타입으로 모두 조회")
void findAllBeanByParentType(){
    Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
    assertThat(beansOfType.size()).isEqualTo(2);
    for(String key : beansOfType.keySet()){
        System.out.println("key = " + key + " value = " + beansOfType.get(key));
    }
}
```
- `getBeansofTyp()`으로 해당 타입 모든 빈 조회 가능
- 하위 타입이 2개가 있기 때문에 2개가 나오는지 테스트
- 스프링 빈 저장소에 Key - value 관계로 저장되는 것을 알 수 있음



<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

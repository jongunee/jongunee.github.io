---
layout: post
title: 스프링 - 객체지향 설계
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 - 객체지향 설계

* toc
{:toc .large-only}

## 스프링 생태계

- __필수:__
  - 스프링 프레임워크
  - 스프링 부트
- __선택:__
  - 스프링 데이터: 데이터베이스 CRUD 지원
  - 스프링 세션: 세션 기능 지원
  - 스프링 시큐리티: 보안
  - 스프링 Rest Docs: 테스트와 엮어 API 문서화
  - 스프링 배치: 배치 처리
  - 스프링 클라우드: 클라우드 기술

## 스프링 프레임워크

- __핵심 기술:__ 스프링 DI 컨테이너, AOP, 이벤트, 기타
- __웹 기술:__ 스프링 MVC, 스프링 WevFlux
- __데이터 접근 기술:__ 트랜잭션, JDBC, ORM 지원, XML 지원
- __기술 통합:__ 캐시, 이메일, 원격접근, 스케줄링
- __테스트:__ 스프링 기반 테스트 지원
- __언어:__ 코틀린, 그루비
- __스프링 부트__ 에서 스프링을 편리하게 사용하도록 지원
- <span style='background-color: #f5f0ff'>스프링은 좋은 객체 지향 애플리케이션을 개발할 수 있게 도와주는 프레임워크</span>

## 객체 지향 프로그래밍 - 다형성

- 컴포넌트를 쉽고 유연하게 변경하며 개발 가능
- 클라이언트는 대상의 역할(인터페이스)만 알면 됨
- 클라이언트는 구현 대상의 내부 구조를 몰라도 됨
- 클라이언트는 구현 대상의 내부 구조가 변경되어도 영향 받지 않음
- 클라이언트는 구현 대상 자체를 변경해도 영향 받지 않음
  - 역할: 인터페이스
  - 구현: 인터페이스를 구현한 클래스, 구현 객체

```java
public class MemberService{
  //Memory 레포지터리 사용
  private MemberRepository memberRepository = new MemoryMemberRepository();
  //JDBC 레포지토리 사용
  private MemberRepository memberRepository = new JdbcMemberRepository();
}
```
코드의 변경 없이 원하는 레포지토리로 사용이 가능  
➡️클라이언트를 손대지 않아도 서버를 변경할 수 있다

## SOLID - 객체 지향 설계 5원칙

1. __SRP(Single Responsibility Principle):__ 단일 책임 원칙
    - 한 클래스는 하나의 책임만 가져야 힘
    - 변경이 있을 떄 파급 효과가 적어야 함
    - ex) UI 변경, 객체 생성과 사용 분리
2. __OCP(Open/Closed Principle):__ 개방-폐쇄 원칙
    - 소프트웨어 요소는 <span style='background-color: #f5f0ff'>확장에는 열려</span> 있으나 <span style='background-color: #f5f0ff'>변경에는 닫혀</span> 있어야 함
    - 다형성 활용
    - 스프링은 DI, DI 컨테이너 제공
3. __LSP(Liskov Substitution Principle):__ 리스코프 치환 원칙
    - 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 함
    - ex) 자동차 인터페이스에서 엑셀 메서드를 구현할 떄 뒤로 가도록 구현되면 LSP 위반
4. __ISP(Interface Segregation Principle):__ 인터페이스 분리 원칙
    - 하나의 범용 인터페이스 보다는 분리
    - 분리해주면 다른 인터페이스에 영향을 주지 않음
    - 인터페이스가 명확해지고 대체 가능성이 높아지고 
5. __DIP(Dependency Invension Principle):__ 의존관계 역전 원칙
    - 구현 클래스에 의존하지 말고 인터페이스에 의존
    - 스프링은 DI, DI 컨테이너 제공





<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

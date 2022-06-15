---
layout: post
title: 스프링 AOP
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 AOP

* toc
{:toc .large-only}

## AOP

- AOP: Aspect Oriented Programming
- 공통 관심 사항(cross-cutting concern) 핵심 관심 사항 (core concern) 분리
- ex) 메서드 호출 시간 측정에 필요


__MemberService 회원 조회 시간 측정__

```java
인프런 스프링 입문 강의 참조

public Long join(Member member) {

    long start = System.currentTimeMillis();

    try{
        validateDuplicateMember(member);    //중복 회원 검증
        memberRepository.save(member);
        return member.getId();
    }finally {
        long finish = System.currentTimeMillis();
        long timeMs = finish - start;
        System.out.println("join = " + timeMs + "ms");
    }
}
```
- 회원 가입, 회원 조회 시간 측정 기능은 핵심 관심 사항이 아님
- 시간 측정 기능은 공통 관심 사항
- 시간 측정 기능은 핵심 비즈니스 로직이 섞여 유지보수가 어려움
- 시간 측정 기능은 공통 로직으로 만들기 어려움
- 시간 측정 기능을 변경할 때 모든 로직을 찾아가며 변경해야 함

![그림1](/assets/img/spring/before_aop.png)  

기존에 AOP가 없다면 각각 시간 측정 로직을 구현해야 했다면

![그림2](/assets/img/spring/after_aop.png)  

AOP를 이용해 시간 측정 로직을 원하는 곳에 적용시킬 수가 있다.

__시간 측정 AOP__

```java
인프런 스프링 입문 강의 참조

@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))")
    public Object execut(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START:" + joinPoint.toString());
        try{
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END:" + joinPoint.toString() + " " + timeMs + "ms");

        }
    }
}
```
- 회원가입, 회원 조회 등의 핵심 관심사항과 시간을 측정하는 공통 관심 사항 분리
- 시간 측정 로직을 별도 공통 로직으로 구현
- 핵심 관심 사항을 깔끔하게 유지
- 변경이 필요하면 이 로직만 변경하면 됨
- 원하는 적용 대상 선택 가능

## AOP 작동

![그림3](/assets/img/spring/di_before_aop.png)  

AOP 이전 의존관계는 `helloController`가 `helloService`를 의존하고 있고 `helloController` 메서드 호출하면 `helloService` 메서드 호출 

![그림4](/assets/img/spring/di_after_aop.png)  

`memberService`에서 AOP가 작동되면 <span style='background-color: #f5f0ff'>프록시</span>라는 가상 `memberService`를 만들어내고 `joinPoint.proceed()` 메서드가 실행되면 실제 `memberService`가 실행됨

![그림5](/assets/img/spring/structure_before_aop.png)  

![그림6](/assets/img/spring/structure_after_aop.png)  




<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
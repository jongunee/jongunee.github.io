---
layout: post
title: 스프링 빈 의존 관계
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 빈 의존 관계

* toc
{:toc .large-only}

## 컴포넌트 스캔을 통한 의존 관계 설정

__MemberController__
```java
인프런 스프링 입문 강의 참조

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    MemberController(MemberService memberService){
        this.memberService = memberService;
    }
}
```
- `@Autowired`: 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.
- 의존관계를 외부에서 넣어주는 것을 <span style='background-color: #f5f0ff'>DI(Dependency Injection) 의존성 주입</span>이라 한다.
- 하지만 memberSerivice가 스프링 빈으로 등록되어 있지 않아 찾을 수 없다

__MemberService 스프링 빈 등록__

```java
인프런 스프링 입문 강의 참조

@Service
public class MemberService {
    private final MemberRepository memberRepository;
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```
__MemberRepository 스프링 빈 등록__

```java
인프런 스프링 입문 강의 참조

@Repository
public class MemoryMemberRepository implements MemberRepository {}
```
- `@Component` 애노테이션을 추가해서 스프링 빈에 자동 등록
-  `@Component`를 포함하는 애노테이션
    - `@Controller`
    - `@Service`
    - `@Repository`
-  위 애노테이션은 `@Component`를 포함하고 있기 때문에 스프링 빈에 자동 등록된다.
- 참고로 처음 시작하는 Main 파일 패키지(hello.hellospring) 하위에 있는 애노테이션만 찾아서 스프링 빈에 등록한다.
- 스프링은 스프링 컨테이너에 스프링 빈을 등록 할 떄 기본적으로 싱글톤으로 등록해서 공유
- 같은 스프링 빈이면 모두 같은 인스턴스

## 자바 코드를 이용한 직접 스프링 빈 등록

__SpringConfig__

```java
인프런 스프링 입문 강의 참조

@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }
    
    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}
```

- 이전 `@Service`, `@Repository`, `@Autowired` 애노테이션 모두 제거 후 진행
 
## DI(Dependency Injection) 의존성 주입
- DI 방식
    - 필드 주입
    - setter 주입
    - 생성자 주입(권장)
- 의존 관계가 실행중에 동적으로 변하는 경우는 거의 없기 떄문에 생성자 주입 권장
- 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔 사용
- 정형화 되지 않거나 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록
- `@Autowired`를 통한 DI는 스프링이 관리하는 객체에서만 동작



<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
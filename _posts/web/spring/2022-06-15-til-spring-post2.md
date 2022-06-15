---
layout: post
title: 스프링 예제 - 회원 도메인
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 예제 - 회원 도메인

* toc
{:toc .large-only}

## 프로젝트 설정

- 스프링 부트 스타터에서 프로젝트 생성
- __Project:__ Gradle Project
- __Spring Boot:__ 2.7.0
- __Group:__ hello
- __Packaging:__ Jar
- __Java:__ 11
- __Dependencies:__ 선택하지 않음

Gradle 라이브러리를 변경할 시에는 코끼리 버튼 또는 우측 Gradle 창에서 Releoad 해주어야 함

## 비즈니스 요구사항 설계

- 회원
  - __기능:__ 회원 가입, 조회
  - __등급:__ 일반, VIP
  - 데이터 레포지터리는 미정
- 주문과 할인 정책
  - 회원은 상품 주문 가능
  - 회원 등급에 따른 할인 정책 적용
  - __할인 정책:__ 모든 VIP에 1000원 고정 금액 할인
  - 할인 정책은 변경 가능성이 높으며 아직 미정

## 회원 도메인 설계

__회원 도메인 협력 관계__

![그림1](/assets/img/spring/member_domain_relationship.png)

- 미확정이기 때문에 회원 저장소 별도 인터페이스 구축
- 우선 임시 개발용으로 메모리 회원 저장소 구현

__회원 클래스 다이어그램__

![그림2](/assets/img/spring/member_class_diagram.png)

__회원 객체 다이어그램__

![그림3](/assets/img/spring/member_object_diagram.png)

- 실제 인스턴스 관계

## 회원 도메인 개발

__회원 등급__

```java
public enum Grade {
    BASIC,
    VIP
}
```

__회원 엔티티__

```java
public class Member {

    private Long id;
    private String name;
    private Grade grade;

    public Member(Long id, String name, Grade grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Grade getGrade() {
        return grade;
    }

    public void setGrade(Grade grade) {
        this.grade = grade;
    }
}

```
- id, name, grade 선언
- 생성자, getter 및 setter

__회원 저장소 인터페이스__

```java
public interface MemberRepository {
    void save(Member member);

    Member findById(Long memberId);
}
```

__회원 서비스 구현 - 메모리 레포지터리__

```java
public class MemoryMemberRepository implements MemberRepository{
    private static Map<Long, Member> store = new HashMap<>();

    @Override
    public void save(Member member) {
        store.put(member.getId(), member);
    }

    @Override
    public Member findById(Long memberId) {
        return store.get(memberId);
    }
}
```
- 실무에선 동시성 이슈 때문에 `HashMap`보다는 `ConcurrentHashMap` 사용 권장

__회원 서비스 인터페이스__

```java
public interface MemberService {

    void join(Member member);

    Member findMember(Long memberId);
}
```

__회원 서비스 구현체__

```java
public class MemberServiceImpl implements MemberService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

## 회원 도메인 - 회원 가입 테스트

```java
//import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.assertj.core.api.Assertions;

public class MemberServiceTest {
    MemberService memberService = new MemberServiceImpl();
    @Test
    void join(){
        //given
        Member member = new Member(1L, "memberA", Grade.VIP);

        //when
        memberService.join(member);
        Member findMember = memberService.findMember(1L);

        //then
        Assertions.assertThat(member).isEqualTo(findMember);
    }
}
```
- `Assertions.assertThat()` 메서드 찾을 수 없으면 core를 import 해야 함
- 인스턴스화 한 `member`와 `join()` 메서드로 등록된 `member(findMember)`가 동일한지 테스트
- 테스트 결과 Good


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

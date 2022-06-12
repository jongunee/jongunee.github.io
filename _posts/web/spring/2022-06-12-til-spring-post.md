---
layout: post
title: 회원 관리 예제 - 설계 및 구현
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: false
hide_last_modified: true
categories:
  - web
  - spring
---

# 회원 관리 예제 - 설계 및 구현

* toc
{:toc .large-only}

## 비즈니스 요구사항
- __데이터:__ 회원 ID, 이름
- __기능:__ 회원 등록, 조회
- __데이터 저장소:__ 아직 선정되지 않음 - 가상 시나리오

![그림 1](/assets/img/spring/web_application_layer.png)

- __컨트롤러:__ 웹 MVC의 컨트롤러 역할
- __서비스:__ 핵심 비즈니스 로직 구현  
    예) 회원은 중복 가입이 안된다는 로직
- __레포지터리:__ 데이터베이스에 접근, 도메인 객체를 DB에 저장 및 관리
- __도메인:__ 비즈니스 도메인 객체,  
    예) 회원, 주문, 쿠폰 등등 데이터베이스에 저장 및 관리됨

![그림2](/assets/img/spring/member_management_ex_class.png)
- 데이터 저장소가 선정되지 않아 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계
- 데이터 저장소는 RDB, NoSQL 등 다양한 저장소 고민 중인 상황
- 개발을 진행하기 위해 초기 개발 단계에서는 구현체로 가벼운 메모리 기반 데이터 저장소 사용 

## 회원 도메인과 레포지터리

__회원 객체__
```java
인프런 스프링 입문 강의 참조

public class Member {
    private Long id;
    private String name;

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
}
```

__회원 레포지터리 인터페이스__
```java
인프런 스프링 입문 강의 참조

public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```
- DB에 멤버 저장
- ID로 찾기
- 이름으로 찾기
- 지금까지 저장된 모든 회원 리스트 반환

__회원 레포지터리 메모리 구현체__
```java
인프런 스프링 입문 강의 참조

public class MemoryMemberRepository implements MemberRepository{
    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
}
```
- 동시성 문제가 고려되어 있지 않음
- 실무에서는 `ConcurrentHashMap`, `AtomicLong` 사용 고려

<span style="font-size:70%">[참조] 인프런 스프링 입문 강의</span>

끝!
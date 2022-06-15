---
layout: post
title: 회원 관리 예제 - 테스트
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 회원 관리 예제 - 테스트

* toc
{:toc .large-only}

## 테스트 케이스

- 개발한 기능을 실행해서 테스트를 하기 위해 `main` 메서드를 통해 실행하거나 웹 어플리케이션의 컨트롤러를 통해 해당 기능 실행
- 이러한 방법은 준비, 실행에 오래 걸림
- 반복 실행이 어려움
- 여러 테스트를 한 번에 실행하기 어려움
- `JUnit` 프레임워크로 테스트 실행 가능

```java
인프런 스프링 입문 강의 참조

class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach(){
        repository.clearStore();
    }
}
```
- \@AfterEach: 테스트는 메서드의 순서에 상관없이 각자 동작
- 독립적으로 실행되어야 하고, 테스트 순서에 의존 관계가 있는 것은 좋은 테스트가 아니다.
- 한 번에 여러 테스트를 하면 테스트 결과가 남을 수 있기 때문에 clear 시켜주는 기능이 필요

__저장 기능 테스트__
```java
@Test
public void save(){
    Member member = new Member();
    member.setName("spring");

    repository.save(member);

    Member result = repository.findById(member.getId()).get();
    assertThat(member).isEqualTo(result);
}
```

__이름 찾기 기능 테스트__

```java
@Test
public void findByName(){
    Member member1 = new Member();
    member1.setName("spring1");
    repository.save(member1);

    Member member2 = new Member();
    member2.setName("spring2");
    repository.save(member2);

    Member result = repository.findByName("spring1").get();
    assertThat(result).isEqualTo(member1);
}
```

__저장된 멤버 찾기 기능 테스트__

```java
@Test
public void findAll(){
    Member member1 = new Member();
    member1.setName("spring1");
    repository.save(member1);

    Member member2 = new Member();
    member2.setName("spring2");
    repository.save(member2);

    List<Member> result = repository.findAll();
    assertThat(result.size()).isEqualTo(2);
}
```
- `assertThat` 메서드를 통해 원하는 결과가 나오는지 확인 가능  

<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
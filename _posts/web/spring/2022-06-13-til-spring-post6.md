---
layout: post
title: 스프링 JPA, 데이터 JPA
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: false
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 JPA, 데이터 JPA

* toc
{:toc .large-only}

## JPA

- 기존의 반복 코드를 줄임
- JPA가 기본 SQL 직접 만들어서 실행
- SQL과 데이터 중심 설계에서 객체 중심 설계 전환
- 개발 생산성을 높임 

__build.gradle - JPA 관련 라이브러리 추가__

```gradle
//implementation 'org.springframework.boot:spring-boot-starter-jdbc'
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```
- jpa 라이브러리 내부에 jdbc 관련 라이브러리를 포함하기 때문에 제거해도 됨

__스프링부트 JPA 설정 추가__

```properties
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none
```

- `resources/application.properties` 경로에 추가
- `show-sql`: JPA가 생성하는 SQL 출력
- `ddl-auto`: JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 `none`을 사용하면 해당 기능을 끔
    - `create`으로 테이벌 직접ㅈ 생성

__JPA 엔티티 매핑__

```java
package hello.hellospring.domain;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Member {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    
    ...
}
```

__JpaMemberRepository__

```java
인프런 스프링 입문 강의 참조

public class JpaMemberRepository implements MemberRepository{

    private final EntityManager em;

    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }

    @Override
    public Member save(Member member) {
        em.persist(member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        Member member = em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
            .setParameter("name", name)
            .getResultList();
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }
}
```

__MemberService에 트랜잭션 추가__
```java
import org.springframework.transaction.annotation.Transactional

@Transactional
public class MemberService {}
```
- 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션 시작
- 메서드가 정상 종료되면 트랜잭션 커밋
- 만약 런타임 예외가 발생하면 롤백
- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행되어야 함

__JpaConfig 스프링 설정__

```java
인프런 스프링 입문 강의 참조

@Configuration
public class SpringConfig {

    private EntityManager em;

    @Autowired
    public SpringConfig(EntityManager em) {
        this.em = em;
    }

    public MemberRepository memberRepository(){
        //return new MemoryMemberRepository();
        //return new JdbcMemberRepository(dataSource);
        //return new JdbcTemplateMemberRepository(dataSource);
        return new JpaMemberRepository(em);
    }
}
```
- 통합테스트도 동일한 코드로 테스트 가능

## 데이터 JPA

- 레포지터리에 구현 클래스 없이 인터페이스 만으로 개발 가능
- 반복 개발한 기본 CRUD 기능 제공

__SpringDataMemberRepository__
```java
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
    Optional<Member> findByName(String name);
}
```
- 스프링 데이터 JPA가 `SpringDataMemberRepository`를 스프링 빈으로 자동 등록
- `findByName()`, `findByEmail()`처럼 메서드 이름만으로 조회 기능 제공

__SpringConfig 스프링 설정 변경__

```java

@Configuration
public class SpringConfig {
    private final MemberRepository memberRepository;

    @Autowired
    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository);
    }
}
```

<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
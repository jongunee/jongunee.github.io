---
layout: post
title: 스프링 JdbcTemplate
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 JdbcTemplate

* toc
{:toc .large-only}

## Jdbc Template

- 순수 Jdbc와 동일한 환경설정
- 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서의 반복 코드 대부분을 제거
- SQL은 직접 작성 필요

__JdbcTemplateMemberRepository__

```java
인프런 스프링 입문 강의 참조

public class JdbcTemplateMemberRepository implements MemberRepository {

  private final JdbcTemplate jdbcTemplate;

  public JdbcTemplateMemberRepository(DataSource dataSource) {
      jdbcTemplate = new JdbcTemplate(dataSource);
  }

  @Override
  public Member save(Member member) {
      SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
      jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id"); Map<String, Object> parameters = new HashMap<>();
      parameters.put("name", member.getName());
      Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
      member.setId(key.longValue());
      return member;
  }

  @Override
  public Optional<Member> findById(Long id) {
      List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
      return result.stream().findAny();
  }

  @Override
  public Optional<Member> findByName(String name) {
      List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);
      return result.stream().findAny();
  }

  @Override
  public List<Member> findAll() {
      return jdbcTemplate.query("select * from member", memberRowMapper() );
  }

  private RowMapper<Member> memberRowMapper(){
      return (rs, rowNum) -> {
          Member member = new Member();
          member.setId(rs.getLong("id"));
          member.setName(rs.getString("name"));
          return member;
      };
  }
}
```

__SpringConfig__
```java
@Bean
public MemberRepository memberRepository(){
    //return new MemoryMemberRepository();
    //return new JdbcMemberRepository(dataSource);
    return new JdbcTemplateMemberRepository(dataSource);
}
```
- JdbcTemplate을 사용하도록 스프링 설정 변경
- 통합테스트도 동일한 코드로 테스트 가능


<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
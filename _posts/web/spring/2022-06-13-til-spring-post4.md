---
layout: post
title: 스프링 통합 테스트
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: false
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 통합 테스트

* toc
{:toc .large-only}

## MemberServiceIntegrationTest

__스프링 컨테이너와 DB까지 연결한 통합 테스트__

```java
인프런 스프링 입문 강의 참조

@SpringBootTest
@Transactional
class MemberServiceInegrationTest {

    @Autowired MemberService memberService;
    @Autowired
    MemberRepository memberRepository;

    @Test
    void 회원가입() {
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        Assertions.assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외(){
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }
}
```
- `@SpringBootTest` : 스프링 컨테이너와 테스트를 함께 실행
- `@Transactional` : 테스트 시작 전에 트랜잭션을 시작하고 테스트 완료 후에 항상 롤백. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지
않음



<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
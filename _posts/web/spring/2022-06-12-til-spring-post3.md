---
layout: post
title: 회원 관리 예제 - 서비스 개발, 테스트
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 회원 관리 예제 - 서비스 개발, 테스트

* toc
{:toc .large-only}

## 서비스 개발

```java
인프런 스프링 입문 강의 참조

public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    //회원 가입
    public Long join(Member member) {
        validateDuplicateMember(member);    //중복 회원 검증
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    //전체 회원 조회
    public List<Member> findMembers(){
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId){
        return memberRepository.findById(memberId);
    }
}
```
- 서비스 관련 코드는 주로 비즈니스와 관련된 용어들을 사용함

## 테스트

```java
인프런 스프링 입문 강의 참조

class MemberServiceTest {

    MemberService memberService;
    MemoryMemberRepository memberRepository = new MemoryMemberRepository();

    @BeforeEach
    public void beforeEach(){
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach(){
        memberRepository.clearStore();
    }
}
```
- `\@BeforeEach`: 설계된 서비스와 테스트에서 `MemoryMemberRepository` 모두 같은 인스턴스를 생성해서 테스트 하도록 한다.
- `\@AfterEach`: 이전 테스트 결과가 남지 않도록 clear

__회원 가입 기능 테스트__

```java
인프런 스프링 입문 강의 참조

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
```
- 테스트 시에는 이름 굳이 영어로 쓸 필요 없음

__중복 회원 확인 기능 테스트__

```java
인프런 스프링 입문 강의 참조

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
    /*try{
        memberService.join(member2);
        fail();
    }catch (IllegalStateException e){
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }*/
    //then
}
```
- 중복되었을 때 같은 예외처리를 하는지 확인
- try - catch로 테스트도 가능

<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
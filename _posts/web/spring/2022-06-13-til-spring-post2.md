---
layout: post
title: 회원 관리 예제 - 웹 MVC
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: false
hide_last_modified: true
categories:
  - web
  - spring
---

# 회원 관리 예제 - 웹 MVC

* toc
{:toc .large-only}

## 회원 웹 기능 - 홈 화면 추가

__HomeController__
```java
인프런 스프링 입문 강의 참조

@Controller
public class HomeController {
    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

__home.html__

```html
인프런 스프링 입문 강의 참조

<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div> <!-- /container -->
</body>
</html>
```
- 컨트롤러가 정적 파일보다 우선 순위가 높기 떄문에 `index.html` 파일이 있더라도 `home.html` 파일을 보여준다

## 회원 웹 기능 - 등록

__MemberController__

```java
인프런 스프링 입문 강의 참조

@GetMapping("/members/new")
public String createForm(){
    return "members/createMemberForm";
}
```

__회원 등록 폼 html__

```html
인프런 스프링 입문 강의 참조

<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <form action="/members/new" method="post">
        <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
        </div>
        <button type="submit">등록</button>
    </form>
</div> <!-- /container -->
</body>
</html>
```
- __경로:__ resources/templates/members/createMemberForm

__웹 등록 화면에서 데이터 전달 받을 폼 객체__

```java
인프런 스프링 입문 강의 참조

public class MemberForm {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

__MemberController에서 회원을 실제 등록하는 기능__


```java
인프런 스프링 입문 강의 참조

@PostMapping("/members/new")
public String create(MemberForm form){
    Member member = new Member();
    member.setName(form.getName());

    memberService.join(member);

    return "redirect:/";
}
```

__회원 가입 동작__
1. `/members/new` url로 들어감
2. `@GetMapping`: Mapping을 통해 `members/createMemberForm` 리턴 
3. templates 아래 `createMemberForm.html`이 뿌러짐
4. \<form> 태그에서 텍스트 값을 입력, name은 키
5. 등록하면 method에 담긴 post 방식을 실행
6. `@PostMapping`: `create` 메서드 호출 해서 `MemberForm`의 `name`에 입력한 텍스트 값이 들어와 넣어줌

## 회원 웹 기능 - 조회

__MemberController에서 조회 기능__

```java
인프런 스프링 입문 강의 참조

@GetMapping("/members")
public String list(Model model) {
    List<Member> members = memberService.findMembers();
    model.addAttribute("members", members);
    return "members/memberList";
}
```

__회원 리스트 html__

```html
인프런 스프링 입문 강의 참조

<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <table>
            <thead> <tr>
                <th>#</th>
                <th>이름</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="member : ${members}">
                <td th:text="${member.id}"></td>
                <td th:text="${member.name}"></td>
            </tr>
            </tbody>
        </table>
    </div>
</div> <!-- /container -->
</body>
</html>
```

- 메모리에 저장을 했기 때문에 웹을 새로 실행하면 저장된 데이터 모두 날라감

<span style="font-size:70%">[참조] 인프런 김영한 - 스프링 입문 강의</span>

끝!
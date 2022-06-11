---
layout: post
title: 정적 컨텐츠, MVC와 템플릿 엔진, API
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: false
hide_last_modified: true
categories:
  - til
  - spring
---

# 정적 컨텐츠, MVC와 템플릿 엔진, API

* toc
{:toc .large-only}

## 정적 컨텐츠

> 정적 컨텐츠란 파일을 그대로 웹브라우저에 전달해 주는 방식

- Spring Boot는 자동으로 정적 컨텐츠 기능 제공
- 대신 그래로 올리는 형태이기 떄문에 프로그래밍은 불가능

예를들어 static 폴더에 `hello-static.html` 파일을 올려놓고 작동시키면 `localhost:8080/hello-static.html`에 그대로 올라온 걸 확인 할 수 있다.

![그림1](/assets/img/spring/static_contents.png)
<span style="font-size:70%">[참조] 인프런 스프링 입문 강의</span>

1. 웹브라우저에서 `localhost:8080/hello-static.html` 호출
2. 내장 톰켓 서버가 요청을 받아 스프링부트에 넘김
3. 컨트롤러가 hello-static 관련된 것이 있는지 확인 (컨트롤러가 우선순위를 가짐)
4. 없다면 스프링부트가 내부 resources에 `static/hello-static.html` 검색
5. 있으면 `hello-static.html` 반환

## MVC와 템플릿 엔진

> 서버에서 프로그래밍을 통해  html을 동적으로 바꿔 내리는 방식. <span style='background-color: #f5f0ff'>MVC란? Model, View, Controller</span>
- 과거에는 View와 Controller가 분리되어 있지 않았음 - Model1 방식

__예제__

Controller
```java
@Controller
public class HelloController {
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

View
```java
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

![그림2](/assets/img/spring/mvc_template_engine.png)
<span style="font-size:70%">[참조] 인프런 스프링 입문 강의</span>

1. 웹브라우저에서 `localhost:8080/hello-mvc` 호출
2. 내장 톰켓 서버가 요청을 받아 스프링부트에 넘김
3. 스프링부트가 hello-mvc와 매핑되어 있는 컨트롤러 호출 
4. 컨트롤러가 `model(key: name, value: spring)`와 `hello-template` return
5. 스프링부트가 viewResolver를 동작
    - ViewResolver: View를 찾아주고 템플릿 엔진 연결
6. return값과 동일한 `templates/hello-template.html`을 찾아 Thymeleaf 템플릿 엔진에 처리 요청
7. 템플릿 엔진이 렌더링을 해서 변환 한 html 파일을 웹 브라우저에 전달

## API

> 안드로이드나 아이폰 클라이언트와 개발할 떄 JSON 형태로 정보를 전달하는 방식

- vue.js나 React도 API 방식 사용
- 서버끼리 통신할 때도 API 방식 사용

__예제__

Controller
```java
@Controller
public class HelloController {
    @GetMapping("hello-string")
    @ResponseBody   //Body부에 데이터를 직접 넣어준다는 의미
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
}
```
- 데이터를 그대로 넣어주어 view 템플릿이 필요 없음

Controller - ResponseBody 객체 반환
```java
public class HelloController {
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name){
        Hello hello = new Hello();
        hello.setName(name);

        return hello;
    }

    static class Hello{
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```

![그림3](/assets/img/spring/api_operation.png)
<span style="font-size:70%">[참조] 인프런 스프링 입문 강의</span>

1. 웹브라우저에서 `localhost:8080/hello-api` 호출
2. 내장 톰켓 서버가 요청을 받아 스프링부트에 넘김
3. 스프링부트가 hello-api와 매핑되어 있는 컨트롤러 호출 
4. @ResponseBody 애너테이션 확인 후 `hello (name: spring)` 객체 넘김
5. `ViewResolver` 대신에 `HttpMessageConverter`가 동작
    - 단순 문자열이면 StringHttpMessageConverter가 동작
    - 객체는 `MappingJackson2HttpMessageConverter` 동작해서 JSON 스타일로 변환
6. JSON 형태로 변환한 `{name: spring}` 웹 브라우저에 전달

참고로 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환타입 정보를 조합해서 `HttpMessageConverter`가 선택된다.

끝!
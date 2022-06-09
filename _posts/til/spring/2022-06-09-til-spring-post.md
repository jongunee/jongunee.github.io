---
layout: post
title: Spring 시작
description: >
  스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 수강 중
sitemap: false
hide_last_modified: true
categories:
  - til
  - spring
---

# Spring 시작

* toc
{:toc .large-only}

## Spring 프로젝트 시작

기존에는 Spring으로 모든 설정을 다 해주며 시작했지만 요즘은 Spring Boot로 빠르게 프로젝트 시작이 가능하다.

- 링크: [Spring initializr 프로젝트 시작](https://start.spring.io)

- Project
    - __Maven Project:__ 주로 예전 레거시 프로젝트들
    - __Gradle Project:__ 요즘 추세
- Spring Boot
    - 2.40(M1), 2.3.2(SNAPSHOT) 등등은 정식 버전이 아님
    - 공식 Release 된 것 중에 최신 버전 사용
- Project Metadata
    - __Group:__ 보통 회사명
    - __Artifact:__ 빌드 결과물 - 프로젝트명
    - __Packaging:__ Jar
- Dependencies
    - 어떤 라이브러리를 사용할 것인지
    - Spring Web, Thymeleaf (html 템플릿 엔진)

__프로젝트 Open__
- Generate해서 다운 받은 후 zip 파일 풀기
- build.gradle 파일 Open

## 프로젝트 파일
- gradle 폴더: gradle 이용을 위한 파일들
- src 폴더
    - main 폴더
        - java: 패키지 및 소스파일 존재
        - resources: 실제 자바코드를 제외한 xml 등의 property 파일
    - test 폴더: 테스트용 코드들 존재
- build.gradle: spring boot에서 제공됨, 버전 설정 및 라이브러리를 가져옴
    - mavenCentral: 설정한 라이브러리를 받아오는 repo 
- Settings - gradle 검색
    - Gradle projects - Gradle로 시작하면 느릴 떄가 있음
        - Build and run using: Intellij IDEA
        - Run tests using: Intellij IDEA

## 프로젝트 시작
- src\main\java\hello.hellospring 경로에 HelloSpringApplication 파일
- main문 실행
- 브라우저에서 `localhost:8080` 접속

## View

__Welcome Page__
- static 경로 아래에 index.html을 올려두면 Welcome page 기능을 제공
- 공식 문서는 다음 링크에서 확인: [Spring Document](https://spring.io/projects/spring-boot)
- __Thymeleaf__ 라는 템플릿 엔진으로 hgml태그에 속성을 추가하고 페이지를 처리할 수 있다.

__Controller__
```java
@Controller
public class HelloController {
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```
- Spring에서 Controller는 @Controller 애너테이션 해주어야 함
- Map 어플리케이션에서 "hello"라고 들어오면 이 메서드를 호출한다는 의미

__hello.html__
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}">안녕하세요. 손님</p>
</body>
</html>
```
- data에 'hello!!'가 담겨서 출력됨

## 동작 환경

![그림1](/assets/img/spring/spring_operation.png)

<span style="font-size:70%">[참조] 인프런 스프링 입문 강의</span>

1. 웹브라우저에서 서버에 `localhost:8080/hello` 전송
2. 스프링부트이 내장하고 있는 톰켓 서버가 받음
3. 스프링 Get 방식으로 `/hello` url과 매칭시켜 `hello()` 메서드 실행
4. 스프링이 model(key: "data", value: "hello!!")을 가지고 "hello" return
5. viewResolver가 `resources\templates` 아래에 {ViewName}.html을 찾음 - hello.html
6. hello.html에서 {data}에 해당되는 값인 "hello!!"가 담겨 결국 "안녕하세요. hello!!"가 화면에 출력

끝!
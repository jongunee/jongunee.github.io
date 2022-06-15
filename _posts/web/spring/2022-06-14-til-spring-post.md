---
layout: post
title: 스프링 개념
description: >
  인프런 스프링부트 개념정리(이론) 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링 개념

* toc
{:toc .large-only}

## 프레임워크 (Framework)

> 영어를 해석해보면 틀안에서 동작한다는 의미.
> 틀을 제공하니 이 틀 안에서 개발을 해라! 

## 오픈 소스

- 소스코드가 공개되어 있음
- 기여가 가능

## IoC 컨테이너를 가짐

>  컨테이너란? 객체의 생명주기 관리, 생성된 인스턴스에 추가적인 기능 제공

- IoC(Inversion of Control): 제어의 역전?  
- 객체관리의 주체가 스프링
- 스프링이 주도권을 가짐

## DI (Dependency Injection) 지원

- DI란 의존성 주입
- 일반적인 프로그래밍은 개발자가 오브젝트의 주소를 관리
- 스프링이 Scan해서 오브젝트를 메모리에 올리기 때문에 손쉽게 사용 가능 ➡️ DI

## 필터

- 스프링은 굉장히 많은 필터를 가지고 있음
- 필터는 문지기 역할
- 톰캣의 필터: web.xml
- 스프링 컨테이너의 필터: 인터셉터

## 어노테이션

> 어노테이션이란 <span style='background-color: #f5f0ff'>주석</span>이지만 컴파일러가 무시하지 않고 <span style='background-color: #f5f0ff'>힌트</span>를 제공한다.

- 스프링은 굉장히 많은 어노테이션을 가지고 있음
- 스프링은 주로 어노테이션을 통해 객체 생성
    - @Component: 클래스를 메모리에 로딩해!
    - @Autowired: 로딩된 객체를 해당 변수에 집어 넣어!
- 리플렉션: 런타임에 클래스 내부의 메서드, 필드, 어노테이션 등 분석 및 설정하는 기법

## MessageConverter

- 스프링 MessageConverter의 기본값은 json 기반
- MessageConverter 기본 라이브러리: json 형태로 변경하는 Jackson
- 예: 자바 프로그램 → json → 파이썬 프로그램

## BuffuredReader/BufferedWriter

- 가변 길이의 문자를 송/수신할 수 있음
- Byte Stream을 통해 통신할 떄 전송 단위가 문자열을 가변 길이로 쓸 수 있게 해주는 클래스
- 하지만 직접 구현할 필요 없이 어노테이션을 사용하면 됨
    - `@ResponseBody`: 송신을 위한 어노테이션으로 `BufferedWriter` 작동
    - `@RequestBody`: 수신을 위한 어노테이션으로 `BufferedReader` 작동
    

<span style="font-size:70%">[참조] 인프런 스프링부트 개념정리(이론)</span>

끝!
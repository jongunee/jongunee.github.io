---
layout: post
title: JPA 개념
description: >
  인프런 스프링부트 개념정리(이론) 수강 중
sitemap: false
hide_last_modified: true
categories:
  - web
  - spring
---

# JPA 개념

* toc
{:toc .large-only}

### JPA란?

> Java Persistence API의 약자로 자바에 있는 데이터를 영구히 기록할 수 있는 환경을 제공하는 API

__API란?__

> Application Programming Interface의 약자로 상하 관계가 존재하는 인터페이스에 따라서 프로그래밍하는 것을 의미한다.

### ORM

> Objects Relational Mapping의 약자로 JPA 인터페이스에 따라 클래스를 모델링을 하여 DB와 매핑하는 것
  
- 오브젝트를 데이터베이스에 연결하는 방법론
- 자바가 DB에 접근해서 조작하는 것을 DML
  - ex) 입력 - Delete, Update, Insert, 출력 - Select
- 자바의 데이터 타입과 DB의 데이터 타입이 다름

### 반복적 CRUD 작업 생략

- 자바가 연결 요청, 세션 열고, 매핑을 하고, 데이터 전송하는 등의 단순 반복적 작업을 함수 하나로 제공
- `Select`, `Select ALl`, `Delete`, `Update`, `Insert` 등의 반복적 작업 생략 가능

### 영속성 컨텍스트

> 컨텍스트는 대상의 모든 정보를 가지고 있다. 데이터를 직접적으로 저장하는 것이 아니라 중간에 있는 영속성 컨텍스트를 거쳐 접근

- 예를들어 DB에 영속성 컨텍스트를 통해 접근해서 데이터를 던져 `Insert`가 호출
- 영속성 컨텍스트에 데이터를 지우면 동기화되어 DB에도 데이터가 사라짐
- 똑같이 영속성 컨텍스트를 통해 데이터를 넣더라도 만약 DB에 있는 데이터가 다르다면 `Insert`가 아니라 `Update`가 호출됨

### DB와 OOP의 불일치성 해결

- ORM을 통해 자바 주도권이 있는 모델 생성 가능
- 자바에서 생성한 객체를 DB에 객체로 저장할 수는 없기 때문에 `Isnert`가 불가능
- JPA를 사용해서 객체를 생성하고 Forign Key로 매핑하여 해결

### OOP 관점의 모델링

- JPA를 통해 상속, 컴포지션 등으로 구현된 클래스를 데이터베이스에 테이블 자동 생성
- 상속: 테이블을 하나 더 생성해서 구현
- 컴포지션: 오브젝트를 가지고 있는 클래스 역시 테이블을 하나 더 생성해서 구현함

### 유지보수성

- 방언 처리 용이
- Migration에 좋음
- 수 많은 DB들이 존재하는데 추상화 객체에 사용하고자 하는 DB를 연동시키면 DB 종류에 상관없이 구헌 가능 

<span style="font-size:70%">[참조] 인프런 스프링부트 개념정리(이론)</span>

끝!
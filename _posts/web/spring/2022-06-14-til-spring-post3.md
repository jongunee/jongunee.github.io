---
layout: post
title: 스트링부트 동작 원리
description: >
  인프런 스프링부트 개념정리(이론) 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 스프링부트 동작 원리

* toc
{:toc .large-only}

## 내장 톰켓

- 스프링은 내장 톰켓을 가지기 때문에 별도로 설치할 필요 없이 바로 실행 가능

__소켓 통신 동작__

- 서버가 포트 오픈
- 클라이언트가 소켓 통신을 통해 서버에 ip주소와 포트번호로 연결
- main 쓰레드는 연결을 담당
- 연결이 되면 추가 쓰레드를 생성해서 동작
- 계속해서 연결되어 있기 때문에 부하가 큼

__http 통신__

- 통신을 끊어버리는 <span style='background-color: #f5f0ff'>Stateless</span> 방식
- 운영체제가 가진 소켓으로 `html` 확장자의 문서를 전달하는 통신
- 운영체제가 가진 기능을 통해 프로그래밍을 <span style='background-color: #f5f0ff'>시스템콜</span>이라고 함
- 클라이언트가 서버에 웹 화면 또는 데이터를 요청하면 반환해주고 연결을 끊어버림
- 지속적으로 쓰레드를 연결을 유지하지 않음
- 부하는 적지만 항상 새로운 연결을 하기 때문에 같은 클라이언트인지 인식할 수 없음 

__웹 서버__

- 클라이언트가 요청시 서버가 응답만 해주는 동작 
- 클라이언트는 IP주소, URL(자원 요청 주소)를 가지고 Request
- 서버는 클라이언트 요청사항에 대해 Response
- ex) Apache

__톰켓 서버__

- 클라이언트가 웹 서버에 데이터 Request
- 웹 서버 (아파치)가 이해하지 못하는 요청(ex) .jsp 자바코드)이 오면 제어권을 톰켓에 전달
- 톰켓은 .jsp 파일을 컴파일하고 컴파일된 데이터를 .html 파일에 덮어 씌워 웹 서버에 반환
- 웹 브라우저가 읽을 수 있도록 html 파일로 만들어주는 역할

## 서블릿 컨테이너

- 클라이언트가 톰캣 서블릿 컨테이너에 요청
- .html, .css, .png 파일 등 요청이 들어오면 톰캣이 아니라 아파치만 작동
- 스프링은 URL 접근을 막아 특정한 파일 요청을 할 수 없기 떄문에 요청시에는 무조건 자바를 거침 ➡️ 톰캣을 거져야 한다
  - URL: 자원 접근  
    ex) http://www.naver.com/a.png
  - URI: 식별자로 접근  
    ex) http://www.naver.com/picture/a
- 최초 요청이면 메모리 로딩하여 서블릿 객체 생성
  - `init()`: 초기화 (최초 호출)
  - `service()`: 어떤 요청인지 체크하며 쓰레드 생성  
    ex) Post, Get, Put, Delete
  - Get 요청이라면 `get()` 메서드 실행
- 최초 요청이 아니면 이미 생성된 객체 재사용
  - 쓰레드만 생성해서 메서드 재호출
  - 쓰레드 개수 제한 가능 - 넘으면 대기

## web.xml

- ServletContext 초기 파라미터
  - 암구호 같은 역할로 어디서든 동작 가능
- Session의 유효시간 설정
- Servlet/JSP에 대한 정의
- Servlet/JSP 매핑
  - 요청한 리소스, 데이터, 위치가 어디인지 알려줌
- Mime Type 매핑
  - Mime Type: 클라이언트가 들고오는 데이터 타입
- Welcome File list
- Error Pages 처리
- 리스너/필터 설정
  - 리스너: 특정 이벤트 발생시 실행되는 메서드나 객체 등
- 보안

## FrontController 패턴
- 최초 앞단에서 request 요청을 받아 필요 클래스에 넘김
- 이 때 새로운 요청이 생김

## requestDispatcher
- 기존에 있던 Request/Response의 리소스가 사라지지 않게 그대로 유지 시킴
- 기존 데이터를 가지고 다른 페이지롱 이동할 때 사용됨

## DispatchServlet

- DispatchServlet = FrontController 패턴 + RequestDispatch
- 스프링에는 DispatchServlet이 있기 때문에 FrontController 패턴을 직접 짜거나 RequestDispatcher를 직접 구현할 필요 없음
- 컴포넌트 스캔
  - 요청을 받아 클래스에 넘기려면 메모리에 로드되어 있어야하기 떄문에 `src`에 모든 파일들을 메모리에 로드
  - @어노테이션으로 구분 - 직접 구현도 가능
- DispatchContext가 컴포넌트 스캔을 할 때 ApplicationContext에 등록 됨

ContextLoaderListner
- 요청에 따라 새로운 쓰레드를 생성
- web.xml에 의해 실행됨

## 스프링 컨테이너

__ApplicationContext__

- 수 많은 객체들이 등록되고 이를 IoC라 함
- 스프링이 직접 객체 관리를 하기 때문에 주소를 알 필요가 없으며 필요할 때 DI를 하면 됨
- 필요한 객체는 모두 ApplicationContext에서 가져올 수 있으며 싱글톤으로 관리되기 때문에 어디에서 접근하든 동일한 객체라는 것이 보장됨
- servlet-applicationContext: 
  - ViewResolver, Interceptor, MultipartResolver 객체 생성
  - @Controller, @RestController 어노테이션 스캔
  - DispatchServlet에 의해 실행
- root-applicationContesxt:
  - 위 어노테이션을 제외한 @Service, @Repository 어노테이션 등 스캔
  - DB관련 객체 생성
  - ContextLoaderListner에 의해 실행
- root-applicationContext가 servlet-applicationContext보다 먼저 로드 됨
- 생성 시점이 다르기 때문에 servlet-applicationContext에서는 root-applicationContext가 로드한 객체를 참조할 수 있지만 그 반대는 불가능

![그림1](/assets/img/spring/webApplicationContext.png)

__Bean Factory__

- 초기에 메모리에 로드되는 것이 아님
- 필요시 `getBean()` 메서드를 통해 호출하여 메모리에 로드 (Lazy-loading)
- 필요할 때 DI하여 사용



## Handler Mapping

- 예를들어 GET 요청으로 해당 주소 요청이 오면 적절한 컨트롤러의 함수를 찾아서 실행

## 응답

- 응답시 html 파일을 응답할지 data를 응답할지 결정 필요
  - html 파일 응답시 ViewResolver가 관여  
  ex) 메서드에서 "hello" return 시 Web-INF/Views/hello.jsp로 리턴하도록 함
  - Data 응답시 MessageConverter가 작동 - json 기반  
  ex) 메서드에 @ResponseBody 어노테이션을 붙여 ViewResolver가 관여하지 않고 데이터로 볼 수 있게 함

__톰캣 동작 과정__

![그림2](/assets/img/spring/tomcat_operation.JPG)
<span style="font-size:70%">[참조]<\_Jbee블로그> https://asfirstalways.tistory.com/334, 2016/11/29</span>



1. web.xml 로딩
2. ContextLoaderListner 생성하여 모든 쓰레드들이 공유하는 것들을 메모리에 띄움
3. root-context.xml 실행 - DB와 관련된 객체들이 컴포넌트 스캔되어 메모리에 로드
4. DB와 관련된 ServiceImpl 동작, DAO, VO 등 생성 후 메모리에 로드
5. 클라이언트로부터 request 요청
6. DispatcherServlet 생성
7. servlet-context 로드
8. Web 과 관련된 Controller 등을 메모리에 로드, 주소 분배
9. 마지막으로 응답시 Data로 리턴할지 html 파일로 리턴할지 정한 뒤 로직 실행


<span style="font-size:70%">[참조] 인프런 스프링부트 개념정리(이론)</span>

끝!
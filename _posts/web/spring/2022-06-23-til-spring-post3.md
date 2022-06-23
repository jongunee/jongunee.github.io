---
layout: post
title: 웹 스코프
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 웹 스코프

* toc
{:toc .large-only}

## 웹 스코프란?

> 웹 스코프란 웹 환경에서만 동작하는 스코프. 프로토타입과 다르게 스프링이 해당 스코프의 종료시점까지 관리하기 때문에 종료 메서드가 호출됨

__웹 스코프 종류__

- __request__ 스코프:  
http 요청 하나가 들어오고 나갈 때까지 유지되는 스코프로 각 http 요청마다 별도의 빈 인스턴스 생성 및 관리
- __session__ 스코프:  
http session과 동일한 생명주기를 가지는 스코프
- __application__ 스코프:  
서블릿 컨텍스트와 동일한 생명주기를 가지는 스코프
- __websocket__ 스코프:  
웹 소켓과 동일한 생명주기를 가지는 스코프

![그림1](/assets/img/spring/http_request_scope.png)

## request 스코프 예제

web 라이브러리 추가

```java
implementation 'org.springframework.boot:spring-boot-starter-web'
```

request 스코프는 동시에 여러 http 요청이 왔을때 정확히 어떤 요청이 남긴 로그인지 구분하기 어려운 경우 사용하기 좋음

- 기대 공통 포멧: [UUID][requestURL]{message}
    - UUID: 사용해 http 요청 구분
    - requestURL: 어떤 url 요청해서 남은 로그인지 확인

__로그 코드__

```java
@Component
@Scope(value = "request")
public class MyLogger {
    private String uuid;
    private String requestURL;

    public void setRequestURL(String requestURL){
        this.requestURL = requestURL;
    }
    public void log(String message){
        System.out.println("[" + uuid + "]" + "[" + requestURL + "] " + message);
    }

    @PostConstruct
    public void init(){
        String uuid = UUID.randomUUID().toString();
        System.out.println("[" + uuid + "] request scope been create: " + this);
    }

    @PreDestroy
    public void close(){
        System.out.println("[" + uuid + "] request scope been close: " + this);
    }
}
```
- `@Scope(value = "request")`: request 스코프로 지정
- `@PostConstruct`: uuid 생성 및 저장 - http 요청 당 하나씩 생성
- `@PreDestory`: 빈이 소멸되는 시점에 종료 메시지를 남김
- `requestURL`: 빈 생성 시점을 알 수 없어 외부에서 setter로 입력 받음

__LogDemoController__

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final ObjectProvider<MyLogger> myLoggerProcider;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request){
        MyLogger myLogger = myLoggerProcider.getObject();
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.logic("testId");

        return "OK";
    }
}
```

- `HttpServiceRequest`: 요청 url 받음
- myLogger에 저장
- 컨트롤러에서 로그 남김

__LogDemoService__

```java
@Service
@RequiredArgsConstructor
public class LogDemoService {
    private final ObjectProvider<MyLogger> myLoggerProvider;
    public void logic(String id){
        MyLogger myLogger = myLoggerProvider.getObject();
        myLogger.log("service id = " + id);
    }
}
```

- 비즈니스 로직이 있는 서비스 계층에서 로그 출력
- request 스코프를 사용하지 않고 파라미터로 넘기면 파라미터가 너무 많아지고 관련 없는 정보까지 넘어감
- MyLogger 멤버 변수에 저장해서 깔끔하게 유지
- `ObjectProvider.getObject()` 메서드:
    - request 스코프 빈의 생성 지연 가능
    - 호출 시점에 http 요청이 진행중이므로 request 스코프 빈 생성 정상 처리 

## 스코프와 프록시

__프록시 방법__

```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyLogger {
}
```
- `proxyMode`:
    - INTERFACES: 적용 대상이 인터페이스일 때
    - TARGET_CLASS: 적용 대상이 인터페이스가 아닌 클래스일 때

__LogDemoController__

Provider 제거

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final MyLogger myLogger;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request){
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.logic("testId");

        return "OK";
    }
}
```

__LogDemoService__

Provider 제거

```java
@Service
@RequiredArgsConstructor
public class LogDemoService {
    private final MyLogger myLogger;
    public void logic(String id){
        myLogger.log("service id = " + id);
    }
}
```
Provider를 제거해도 동일한 동작을 한다

MyLogger의 가짜 프록시 클래스를만들어 http 요청과 상관없이 가짜 프록시 클래스를 다른 빈에 미리 주입

- `@Scope` 프록시 설정시 스프링 컨테이너는 `CGLIB`라는 라입브러리로 바이트 코드 조작
- MyLogger 상속 받아 가짜 프록시 생성
- 스프링 컨테이너에 가짜 프록시 객체 등록 

![그림2](/assets/img/spring/proxy.png)

- 가짜 프록시 객체는 진짜 빈을 요청하는 위임 로직이 들어있음
- 가짜 프록시 객체는 request 스코프의 진짜 `myLogger.logic()` 호출


<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

---
layout: post
title: 빈 생명주기 콜백 
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 빈 생명주기 콜백

* toc
{:toc .large-only}

## 빈 생명주기 콜백 시작

필요 연결을 미리 해두거나 애플리케이션에 연결을 모두 종료하는 작업을 진행하려면 객체의 초기화와 종료 작업 필요  
ex) 데이터비에스 커넥션 풀, 네트워크 소켓

__NetworkClient 코드__

```java
public class NetworkClient {
    private String url;

    public NetworkClient(){
        System.out.println("생성자 호출, url = " + url);
        connect();
        call("초기화 연결 메시지");
    }

    public void setUrl(String url) {
        this.url = url;
    }

    //서비스 시작시 호출
    public void connect(){
        System.out.println("connet: " + url);
    }

    public void call(String message){
        System.out.println("call: " + url + " message = " + message);
    }

    //서비스 종료시 호출
    public void disconnect(){
        System.out.println("close: " + url);
    }
}
```

__테스트 코드__

```java
public class BeanLifeCycleTest {
    @Test
    public void lifeCycleTest(){
        ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        NetworkClient client = ac.getBean(NetworkClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig{
        @Bean
        public NetworkClient networkClient(){
            NetworkClient networkClient = new NetworkClient();
            networkClient.setUrl("http://hello-spring.dev");

            return networkClient;
        }
    }
}
```

객체 생성 단계에 `url`이 없기 때문에 `url`에 `null` 값이 들어간다.

__싱글톤 스프링 빈 이벤트 라이프 사이클__

스프링 컨테이너 생성 → 스프링 빈 생성 → 의존관계 주입 → 초기화 콜백 → 사용 → 소멸전 콜백 → 스프링 종료


__객체 생성과 초기화 분리__

- 생성자: 필수 정보(파라미터)를 받고 메모리를 할당해 객 생성
- 초기화: 생성된 값을 활용해 외부 커넥션을 연결하는 등의 무거운 동작 수행
- 생성자 안에서 무거운 동작까지 하기보다는 객체 생성과 초기화를 분리시키는 것이 유지보수에 좋음 

## 인터페이스 빈 생명주기 콜백

__NetworkClient 코드__

```java
public class NetworkClient implements InitializingBean, DisposableBean {
    ...
    @Override
    public void afterPropertiesSet() throws Exception {
        connect();
        call("초기화 연결 메시지");
    }

    @Override
    public void destroy() throws Exception {
        disconnect();
    }
}
```
- `InitializaingBean`:
  - `afterPropertiesSet()` 메서드로 초기화 지원
  - 생성자 생성된 뒤 호출
- `DisposableBean`:
  - `destroy()` 메서드로 소멸 지원
  - 스프링 컨테이너 종료 호출되고 소멸 메서드 호출
- 스프링 전용 인터페이스로 이에 의존
- 초기화, 소멸 메서드 이름 변경 불가능
- 코드를 고칠 수 없는 외부라이브러리에 적용 불가능
- 초창기에 나온 방식이며 거의 사용하지 않음 

## 빈 등록 초기화, 소멸 메서드

__NetworkClient 코드__

```java
public class NetworkClient {
    ...
    public void init() {
        connect();
        call("초기화 연결 메시지");
    }

    public void close() {
        disconnect();
    }
}
```

__테스트 코드__

```java
@Configuration
static class LifeCycleConfig{
    @Bean(initMethod = "init", destroyMethod = "close")
    public NetworkClient networkClient(){
        NetworkClient networkClient = new NetworkClient();
        networkClient.setUrl("http://hello-spring.dev");

        return networkClient;
    }
}
```
- 간단하게 `@Bean(initMethod = "init", destroyMethod = "close")`처럼 지정 가능
- 메서드 이름을 자유롭게 줄 수 있음
- 스프링 빈이 스프링 코드에 의존하지 않음
- 코드가 아니라 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 적용 가능 
- `@Bean`의 `destroyMethod` 속성
  - 라이브러리는 대부분 `close`, `shutdown`이라는 이름의 종료 메서드를 사용
  - 따라서 직접 등록하지 않더라도 해당 메서드가 있다면 추로해서 자동으로 호출
  - 추론 기능을 사용하지 않으려면 `destoryMethod=""`으로 빈 공백 지정

## 애너테이션 @PostConstruct, @PreDestory

__NetworkClient 코드__

```java
public class NetworkClient {
    private String url;

    @PostConstruct
    public void init() {
        connect();
        call("초기화 연결 메시지");
    }

    @PreDestroy
    public void close() {
        disconnect();
    }
}
```
- 편리하며 스프링에서 가장 건장하는 방법
- `javax` 패키지에 속해 있다 ➡️자바 표준이기 때문에 다른 컨테이너를 사용해도 적용 가능
- 외부 라이브러리에는 적용 불가능하기 때문에 외부 라이브러리를 사용해야 한다면 `@Bean` 기능을 사용하면 됨




<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

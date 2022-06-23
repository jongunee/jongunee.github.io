---
layout: post
title: 싱글톤, 프로토타입 스코프 빈 함께 사용
description: >
  인프런 스프링 핵심 원리 - 기본편 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - spring
---

# 싱글톤, 프로토타입 스코프 빈 함께 사용

* toc
{:toc .large-only}

## 함께 사용할 때 문제점

### 프로토타입 빈 직접 요청

![그림1](/assets/img/spring/prototype_bean_direct_request1.png)

1. 클라이언트A가 스프링 컨테이너에 프로토타입 빈 요청
2. 스프링 컨테이너는 프로토타입 빈을 새로 생성해 반환, 필드값 0
3. 클라이언트A가 조회한 프로토타입 빈에 `addCount()` 호출해 필드값 + 1
4. 프로토타입 빈의 필드값은 1 

![그림2](/assets/img/spring/prototype_bean_direct_request2.png)

1. 클라이언트B가 스프링 컨테이너에 프로토타입 빈 요청
2. 스프링 컨테이너는 프로토타입 빈을 새로 생성해 반환, 필드값 0
3. 클라이언트가 조회한 프로토타입 빈에 `addCount()` 호출해 필드값 + 1
4. 역시 프로토타입 빈의 필드값은 1

__코드 테스트__

```java
public class SingletonWithPrototypeTest1 {
    @Test
    void prototypeFind(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
        PrototypeBean prototypeBean1 = ac.getBean(PrototypeBean.class);
        prototypeBean1.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);

        PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class);
        prototypeBean2.addCount();
        assertThat(prototypeBean2.getCount()).isEqualTo(1);

    }

    @Scope("prototype")
    static class PrototypeBean{
        private int count = 0;

        public void addCount(){
            count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init(){
            System.out.println("PrototypeBean.init = " + this);
        }

        @PreDestroy
        public void destroy(){
            System.out.println("PrototypeBean.destory");
        }

    }
}
```
두 프로토타입 빈의 필드값 모두 1

### 싱글톤 빈에서 프로토타입 빈 사용

- `clientBean`이라는 싱글톤 빈에서 프로토타입 빈을 주입받아 사용
- `clientBean`은 싱글톤이기 때문에 스프링 컨테이너 생성 시점에 함께 생성 및 의존관계 주입 발생

![그림3](/assets/img/spring/singleton_prototype_bean1.png)

1. `clientBean`은 의존관계 자동 주입, 주이 시점에 스프링 컨테이너에 프로토타입 빈 요청
2. 스프링 컨테이너가 프로토타입 빈 생성 후 `clientBean`에 반환, 프로토타입 빈 필드값 0
3. `clientBean`은 프로토타입 빈을 내부 필드에 보관

![그림4](/assets/img/spring/singleton_prototype_bean2.png)

4. 클라이언트A가 스프링 컨테이너에 `clientBean` 요청
    - 싱글톤이기 때문에 항상 같은 `clientBean` 반환
5. 클라이언트A가 `clientBean.logic()` 호출
6. `clientBean`은 `prototypeBean`의 `addCount()`를 호출해서 프로토타입 빈의 필드값 + 1

![그림5](/assets/img/spring/singleton_prototype_bean3.png)

7. 클라이언트A가 스프링 컨테이너에 `clientBean` 요청
    - 싱글톤이기 때문에 항상 같은 `clientBean` 반환
    - `clientBean`이 내부에 가지고 있는 프로토타입 빈은 이미 주입이 끝난 빈
    - 주입 시점에 스프링 컨테이너에 요청해서 프로토타입 빈이 생성되기 때문에 새로운 생성이 되는 것은 아님
8. 클라이언트B가 `clientBean.logic()` 호출
9. `clientBean`은 내부 `prototypeBean`의 `addCount()`를 호출해서 프로토타입 빈의 필드값 + 1 되어 2가 됨

__코드 테스트__

```java
public class SingletonWithPrototypeTest1 {
    ...
    @Test
    void singletonClientUsePrototype(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(ClientBean.class, PrototypeBean.class);
        ClientBean clientBean1 = ac.getBean(ClientBean.class);
        int count1 = clientBean1.logic();
        assertThat(count1).isEqualTo(1);

        ClientBean clientBean2 = ac.getBean(ClientBean.class);
        int count2 = clientBean2.logic();
        assertThat(count2).isEqualTo(2);
    }

    @Scope("singleton")
    static class ClientBean{
        private final PrototypeBean prototypeBean; //생성시점에 주입

        @Autowired
        public ClientBean(PrototypeBean prototypeBean) {
            this.prototypeBean = prototypeBean;
        }

        public int logic(){
            prototypeBean.addCount();
            return prototypeBean.getCount();
        }
    }

    @Scope("prototype")
    static class PrototypeBean{
        private int count = 0;

        public void addCount(){
            count++;
        }
        ...
    }
}
```
프로토타입 빈의 필드값이 증가함

프로토타입 빈의 사용 목적은 항상 새로운 빈을 생성하는 것이기 때문에 의도한 것과 다른 결과가 나온다

## 문제점 해결 - Provider

__스프링 컨테이너에 요청__

```java
@Autowired
private ApplicationContext ac;

public int logic() {
    PrototypeBean prototypeBean = ac.getBean(PrototypeBean.class);
    prototypeBean.addCount();
    int count = prototypeBean.getCount();
    return count;
}
```

- `ac.getBean()`을 통해 항상 새로운 프로토타입이 생성
- 의존관계를 외부에서 주입받는 DI가 아니라 직접 필요한 의존관계를 찾는 DL(Dependency Lookup) 의존관계 조회(탐색)
- 스프링 컨테이너에 종속적
- 단위테스트에 어려움

__ObjectFactory, ObjectProvider__

```java
static class ClientBean{

    @Autowired
    private ObjectProvider<PrototypeBean> prototypeBeanProvider;

    public int logic(){
        PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
        prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```
- `prototypeBeanProvider.getObject()`를 통해 항상 새로운 프로토타입 빈이 생성됨
- `getObject()` 호출시 스픵 컨테이너를 통해 해당 빈을 찾아 반환 (DL)
- 단위테스트나 mock 코드 만들기 쉬움
- `ObjectProvider`는 `ObjectFactory`를 상속받아 편의 기능이 추가된 것

__JSR-330 Provider__

JSR-330 자바 표준을 사용하기 위해 라이브러리를 gradle에 추가해야 한다

```java
javax.inject:javax.inject:1
```

__테스트 코드__

```java
@Scope("singleton")
static class ClientBean{

    @Autowired
    private Provider<PrototypeBean> prototypeBeanProvider;

    public int logic(){
        PrototypeBean prototypeBean = prototypeBeanProvider.get();
        prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```
- 선언 해주고 메서드명만 바꾸어주면 동일 동작
- `get()` 메서드 하나로 기능 매우 단순
- 자바 표준이므로 스프링이 아닌 다른 컨테이너에서 사용 가능



<span style="font-size:70%">[참조] 인프런 스프링 핵심 원리 - 기본편 - [링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)</span>

끝!

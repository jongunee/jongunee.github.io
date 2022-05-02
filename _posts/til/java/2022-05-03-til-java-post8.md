---
layout: post
title: 인터페이스
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
mermaid: true
categories:
  - til
  - java
---
## 인터페이스

* toc
{:toc .large-only}

## 인터페이스란?
> 모든 메서드가 추상 메서드로 이루어진 클래스를 의미하며 형식저인 선언만 있고 구현은 없다.
>- 인터페이스에 모든 메서드는 추상 메서드로 `public abstract`로 선언한다
>- 인터페이스에 모든 변수는 상수로 `public static final`로 선언한다

__예시__
```java
interface 인터페이스 이름{
  public static final float pi = 3.14f;
  public void add();
}
```
<mark>인터페이스의 모든 추상 메서드를 구현하지 않으면 추상 클래스가 된다.</mark>

__추상클래스 VS 인터페이스__
- 인터페이스는 `interface` 키워드를 사용해 선언하고 `extends` 키워드를 사용해 <span style='background-color: #f5f0ff'>다중 상속</span>을 구현한다. 
- "is able to"로 표현하며 "클래스가 무엇을 할 수 있다"라고 하는 <span style='background-color: #f5f0ff'>공통된 기능</span>을 구현하도록 강제하는 것

---

## 인터페이스 구현과 형 변환

- 인터페이스로 구현한 클래스는 인터페이스 형으로 선언한 변수로 형 변환 할 수 있다.
- 상속에서의 형 변환과 동일한 의미를 가진다.
- 형 변환시 사용할 수 있는 메서드는 인터페이스에 선언된 메서드만 사용할 수 있다.
- 클래스 상속과 달리 구현 코드가 없기 때문에 여러 인터페이스를 구현할 수 있다.

```java
public class CompleteCalc implements Calc, Runnable{ // 여러 implements 구현 가능
  ...
}
```

---

## 인터페이스와 다형성
- 인터페이스는 <span style='background-color: #f5f0ff'>클라이언트 코드</span>와 서비스를 제공하는 <span style='background-color: #f5f0ff'>객체</span> 사이의 약속
- 어떤 객체가 `interface` 타입이라는 것은 그 `interface`가 제공하는 메서드를 구현했다는 의미
- 클라이언트는 어떻게 구현되었는지 상관 없이 `interface`의 정의만을 보고 사용할 수 있음  
ex) JDBC
- 다양한 구현이 필요한 인터페이스를 설계하는 것은 매우 중요한 일

---

## 인터페이스 요소
- __상수:__ 모든 변수는 상수로 변환 됨
- __추상 메서드:__ 모든 메서드는 추상 메서드로 구현 됨
- __default 메서드:__ 기본 구현을 가지는 메서드로 구현 클래스에서 재정의 할 수 있음
- __static 메서드:__ 인스턴스 생성과 상관 없이 인터페이스 타입으로 사용할 수 있는 메서드
- __private 메서드:__ 인터페이스를 구현한 클래스에서 사용하거나 재정의 할 수 없음. 인터페이스 내부에서만 기능을 제공하기 위해 구현하는 메서드

__default 메서드 재정의__
```java
public interface Calc{
  ...
  default void description(){
    System.out.println("정수 계산기 구현"); //Java8 부터 제공
  }
}
```

```java
public class CompleteCalc extends Calculator{
  ...
  @override
  public void dexcription(){
    System.out.println("완전한 계산기 구현");  // default 메서드 언하는 기능으로 재정의 
  }
}
```

__static 메서드__
```java
public interface Calc{
  ...
  static int total(int[] arr){
    int total = 0;
    for(int i : arr){
      total += i;
    }
  }
  return total;
}
```

```java
int[] arr = {1, 2, 3, 4, 5};
int sum = Calc.total(arr);  // 인스턴스 생성 없이 사용 가능
```

__private 메서드__  
인터페이스 내부에서 `private` 혹은 `private static`으로 선언한 메서드를 구현한다. 
```java
public interface Calc{
  ...
  default void description(){
    System.out.println("정수 계산기 구현");
    myMethod(); // default 메서드에서 private 메서드 호출
  }


  static int total(int[] arr){
    int total = 0;
    for(int i : arr){
      total += i;
    }
    myStaticMethod(); // static 메서드에서 private static 메서드 호출
  }
  return total;
}
```
```java
private void myMethod(){
  System.out.println("private 메서드 호출");
}
```
```java
private static void myStaticMethod(){
  System.out.println("private static 메서드 호출");
}
```

---

## 두 개의 인터페이스 구현

__Buy 인터페이스__
```java
public interface Buy{
  void buy();
}
```

__Sell 인터페이스__
```java
public interface Sell{
  void sell();
}
```

__Customer 인터페이스__
```java
public class Customer implements Buy, Sell{
  @Override
  public void buy(){
    System.out.println("구매하기");
  }

  @Override
  public void sell(){
    System.out.println("판매하기");
  }
}
```
__테스트__
```java
Customer customer = new Customer(); //구매, 판매 모두 가능
customer.buy();
customer.sell();

Buy buyer = customer;
buyer.buy();  // 구매만 가능

Sell seller = customer;
seller.sell();  // 판매만 가능
```

두 개의 인터페이스로부터 implements 했을 경우 같은 이름의 default 메서드가 있을 수 있다. 그럴 때는 구현 시 오버라이딩 해주어야 한다.

__Buy 인터페이스__
```java
public interface Buy{
  void buy();

  default void order(){
    System.out.println("구매 주문");
  }
}
```

__Sell 인터페이스__
```java
public interface Sell{
  void sell();

  default void order(){
    System.out.println("판매 주문");
  }
}
```

__Customer 인터페이스__
```java
public class Customer implements Buy, Sell{
  @Override
  public void buy(){
    System.out.println("구매하기");
  }

  @Override
  public void sell(){
    System.out.println("판매하기");
  }

  @Override
  public void order(){  // 메서드의 이름이 동일하기 때문에 오버라이딩이 필요함
    System.out.println("고객 판매 주문");
  }
}
```

__테스트__
```java
Customer customer = new Customer();
Buy buyer = customer;
Sell seller = customer;

customer.order(); //고객 판매 주문 출력
buyer.order();  //고객 판매 주문 출력
seller.order(); //고객 판매 주문 출력
```
Java는 모드 가상 메서드 방식이기 때문에 오버라이딩 된 인스턴스의 메서드가 출력이 된다.

---

## 인터페이스 상속  

인터페이스 간에도 상속이 가능한 데 구현 코드의 상속이 아니기 때문에 형 상속(type inheritance) 라고 한다.

인터페이스 구현과 클래스 상속을 함께 사용하는 것도 가능하다.

__예시__
```java
public class BookShelf extends Shelf implements Queue{
  ...
}
```
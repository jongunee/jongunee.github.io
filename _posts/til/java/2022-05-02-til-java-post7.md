---
layout: post
title: 추상 클래스
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
mermaid: true
categories:
  - til
  - java
---
## 추상 클래스

* toc
{:toc .large-only}

## 추상 클래스란? (abstract class)

> 추상 메서드를 포함한 클래스를 의미한다. 추상 메서드는 구현 코드 없이 선언만 있는 메서드를 의미하며 `abstract` 예약어를 사용한다. 추상 클래스는 `new` (인스턴스화) 할 수 없다.

__예시__
```java
abstract int add(int x, int y); // 선언만 있는 추상 메서드
int add(int x, int y){}  // 내용은 없지만 {}로 구현이 됐기 때문에 추상 메서드가 아니다.
```

__추상 클래스 구현__  
- 메서드에 구현 코드가 없으면 `abstact`로 선언해야 한다.
- `abstract`로 선언된 메서드를 가진 클래스는 `abstract`로 선언해야 한다.
- 모든 메서드가 구현 코드가 있더라도 클래스가 `abstract`로 선언되었다면 추상 클래스가 된다.  
➡️ new 할 수 없다.

```java
public abstract class Computer{
  public abstract void display();
  public abstract void typing();
  ...
}
```

<mark>추상클래스에는 구현되지 않은 추상 메서드를 가지고 있기 때문에 추상 클래스를 상속받은 하위 클래스는 추상 메서드를 구현해서 실체클래스로 사용해야한다.</mark> 추상 클래스는 미완성 설계도라고 할 수 있다.

---

## 추상 클래스와 템플릿 메서드

__템플릿 메서드__  
> <span style='background-color: #f5f0ff'>템플릿 메서드 패턴</span>은 프레임워크에서 많이 사용되는 <span style='background-color: #f5f0ff'>디자인 패턴</span> 중 하나이다. 추상 클래스로 선언된 상위 클래스에 <span style='background-color: #f5f0ff'>템플릿 메서드</span>를 활용해서 전체적인 흐름을 정의하고 하위 클래스에서 다르게 구현되어야 하는 부분은 <span style='background-color: #f5f0ff'>추상 메서드</span>로 선언해서 하위 클래스가 구현하도록 설계하는 방식이다.  
> 여기서 <span style='background-color: #f5f0ff'>템플릿 메서드</span>는 추상 메서드나 구현된 메서드를 활용해서 전체 기능의 흐름(시나리오)을 정의하는 메서드로 `final`로 선언하여 하위 클래스에서 재정의 할 수 없도록 한다.

__예시 - 자동차 시나리오__
```java
final public void run(){
  startCar();
  drive();
  wiper();
  stop();
  washCar();
  turnoff();
}
```

__후크(Hook) 메서드__
> `abstract` 키워드를 붙이면 상속 받은 클래스가 반드시 해당 추상 메서드를 구현해야한다. 하지만 `abstract` 키워드 없이 후크 메서드로 만들면 반드시 구현할 필요가 없고 선택적으로 오버라이딩 할 수 있다.

__예시__
```java
public void washCar() {}
```
구현만 하고 코드 내용을 입력하지 않기 때문에 선택적 오버라이딩이 가능하다.

---

## final 예약어
- `final` 변수는 값이 변경될 수 없는 상수이다.
```java
    public static final double PI = 3.14;
```
- `final` 변수는 오직 한 번만 값을 할당할 수 있다.
- `final` 메서드는 하위 클래스에서 오버라이딩 할 수 없다.
- `final` 클래스는 더 이상 상속되지 않는다.  
  ex) Java에서 `String` 클래스

__final 활용__  
프로젝트를 구현할 때 여러 파일에서 공유해야 하는 상수 값은 하나의 파일에 선언해서 사용하면 편리하다.

__예시__
```java
public class Define{
  public static final int MIN = 1;
  public static final int MAX = 99999;
  public static final int ENG = 1001;
  public static final int MATH = 2001;
  public static final double double PI = 3.14;
  public static final String GOOD_MORNING = "Good Morning!";
}
``` 
---
layout: post
title: 상속과 다형성
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
mermaid: true
categories:
  - til
  - java
---
## 상속

* toc
{:toc .large-only}

## 상속

__상속이란?__
> 클래스를 정의 할 때 이미 구현된 클래스를 상속 받아 속성이나 기능이 확장되는 클래스를 구현할 수 있다. 상속을 통해 다형성을 구현할 수가 있으며 유지보수에 유용하다.
> - 상위 클래스는 하위 클래스보다 일반적인 의미를 가진다.
> - 하위 클래스는 상위 클래스보다 구체적인 의미를 가진다.
> - 상속하는 클래스: 상위 클래스, parent class, base class, super class
> - 상속받는 클래스: 하위 클래스, child class, derived, class, subclass  



<mark>상속에 코드의 재사용이 되긴하지만 코드의 재사용 방법이 상속인 것은 아니다.</mark> 예를 들어, Point 좌표 클래스와 원 좌표 클래스가 있다고 가정하면 원을 그릴 때 Point가 사용되지만 상속의 관계는 아니다.

__상속 선언__
```java
class B extends A{
  ...
}
```
➡️ A 클래스가 B 클래스에게 상속한다 == B 클래스가 A 클래스를 상속받는다.

__예시__

```java
class Mammal{

}

class Human extends Mammal{

}
```
<mark>자바는 Single inheritance만을 지원하기 때문에 extends 뒤에는 단 하나의 class만 사용할 수 있다.</mark> 예를 들어, 고객관리 프로그램에서 등급에 따라 서비스를 제공하기 위해 일반적인 Customer 클래스와 VIP/Gold 등급 Customer 클래스 구현할 때 사용될 수 있다.

---

## protected 키워드

> 상위 클래스에서 `private` 키워드를 사용하면 하위 클래스에서 `private` 키워드로 선언된 변수에 접근할 수가 없다. 하지만 상위 클래스에서 `protected` 키워드를 사용해서 선언하면 하위 클래스에서 접근이 가능하다. 즉, 상속 관계에서 가시성이 있게된다.

__접근 제한자 가시성__

|  | <center>외부 클래스</center> | <center>하위 클래스</center> | <center>동일 패키지</center> | <center>내부 클래스</center> |
| ------ | ------ | ------ | ------ | ------ |
| public | <center>O</center> | <center>O</center> | <center>O</center> | <center>O</center> |
| protected | <center>X</center> | <center>O</center> | <center>O</center> | <center>O</center> |
| default | <center>X</center> | <center>X</center> | <center>O</center> | <center>O</center> |
| private | <center>X</center> | <center>X</center> | <center>X</center> | <center>O</center> |

---

## 상속에서 클래스 생성 과정

- 하위 클래스가 생성 될 때 상위 클래스가 먼저 생성이 된다.
- 상위 클래스의 생성자가 호출되고 하위 클래스의 생성자가 호출된다.
- 하위 클래스의 생성자에서는 무조건 상위 클래스의 생성자가 호출되어야 한다.
- 아무것도 없는 경우 컴파일러는 상위 클래스의 기본 생성자를 호출하기 위해 `super()`를 코드에 넣어준다.
- `super()` 호출되는 생성자는 상위 클래스의 기본 생성자이다.
- 만약 상위 클래스의 기본 생성자가 없는 경우(매개변수가 있는 생성자만 존재하는 경우) 하위 클래스는 명시적으로 상위 클래스를 호출해야 한다.


__예시 1__
```java
public customer(){
  super(); // 상위 클래스도 super 생성자가 생성된(모든 클래스는 최상위의 object 클래스로부터 상속 받음)
}

public VIP customer(){
  super();  // 따로 선언해주지 않더라도 컴파일러가 기본 생성자를 넣어준다.
  custoemrGrade = "VIP";
  bonusRatio = 0.05; 
  salesRatio = 0.1; 
}
```

__예시 2__
```java
public Customer(int customerID, String customerName){
  this.customerID = customerID;
  this.customerName = customerName;
  customerGrade = "Silver";
  bonusRatio = 0.01;
}

public VIPCustomer(int customerID, String customerName){
  super(customerID, customerName); // 상위 클래스 생성자를 호출할 수 없기 때문에 직접 지정해주어야 함
  customerGrade = "VIP";
  bonusRatio = 0.05;
  salesRatio = 0.1; 
}
```

__super 예약어__
> `this`가 자기 자신의 인스턴스 주소를 가지는 것처럼 `super`는 하위 클래스가 상위 클래스에 대한 주소를 갖게 된다. 하위 클래스가 상위 클래스에 접근할 때 사용할 수 있다.

---

## 상위 클래스로의 묵시적 형 변환

상위 클래스 형으로 변수를 선언하고 하위 클래스 인스턴스를 생성할 수 있다. 하위 클래스는 상위 클래스의 타입을 내포하고 있기 때문에 상위 클래스로 묵시적 형변환이 가능하다. 이를 업캐스팅이라고도 한다.

```java
상위 클래스형 클래스이름 = new 하위 클래스형();
Customer vc = new VIPCustomer();
```
<mark>vc는 Customer 형으로 선언되었기 때문에 VIPCustomer의 멤버 변수나 메서드는 확인할 수 없다.</mark>


끝!
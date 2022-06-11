---
layout: post
title: 오버라이딩과 다형성
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
mermaid: true
categories:
  - web
  - java
---
# 오버라이딩, 다형성

* toc
{:toc .large-only}

## 메서드 오버라이딩
> 상위 클래스에 정의 된 메서드 중 하위 클래스와 기능이 맞지 않거나 추가 기능이 필요한 경우 같은 이름과 매개변수로 하위 클래스에서 <span style='background-color: #f5f0ff'>재정의</span> 할 수 있다.

__예시__
```java
public int calcPrice(int price){  //일반 Customer 클래스(상위 클래스) 메서드
  bonusPoint += price * bonusRatio;
  return price;
}

public int calcPrice(int price){  // VIPCustomer 클래스(하위 클래스) 메서드 재정의
  bonusPoint += price * bonusRatio; // VIP bonusRatio 적용
  return price - (int)(price * salesRatio); // 가격 할인
}
```

---

## 가상 메서드(virtual method)
> 프로그램에서 어떤 객체의 변수나 메서드의 참조는 그 타입에 따라 이루어진다. <span style='background-color: #f5f0ff'>가상 메서드</span>의 경우는 타입과 상관 없이 실제 생성된 인스턴스의 메서드가 호출되는 원리이다.

아래 코드에서는 어떤 <span style='background-color: #f5f0ff'>메서드</span>가 호출 될까?
```java
Customer vc = new VIPCustomer();
vc.calcPrice(10000);
```
➡️ 재정의된 `VIPCustomer` 클래스의 `calcPrice()` 메서드가 호출된다. 만약 오버라이딩을 하지 않았다면 `Customer` 클래스의 메서드가 호출된다.  
➡️즉, vc 타입은 `Customer`지만 실제 생성된 인스턴스인 `VIPCustoemr` 클래스의 `calcPrice()`메서드가 호출된다. 

<mark>C++에서는 virtual 키워드를 사용해야 하지만 Java에서는 기본적으로 모든 메서드가 가상 메서드이다.</mark>

---

## 다형성
__다형성 이란?__
> 하나의 코드가 여러가지 자료형으로 구현되어 실행되는 것을 의미히며 <span style='background-color: #f5f0ff'>정보은닉</span>, <span style='background-color: #f5f0ff'>상속</span>과 더불어 객체지향 프로그래밍의 가장 큰 특징 중 하나이다. 객체지향 프로그래밍의 <span style='background-color: #f5f0ff'>유연성, 재활용성, 유지보수성</span>에 기본이 되는 특징이다.

__예시 - 오버라이딩__
```java
class Animal{
  public void move(){
    System.out.println("동물이 움직입니다.");
  }
}

class Human extends Animal{
  public void move(){
    System.out.println("사람이 두발로 걷습니다.");
  }
}

class Tiger extends Animal{
  public void move(){
    System.out.println("호랑이가 네발로 뜁니다.");
  }
}

class Eagle extends Animal{
  public void move(){
    System.out.println("독수리가 하늘을 납니다.");
  }
}
```

__예시 - main문__
```java
public static void main(String[] args){
  AnimalTest test = new AnimalTest();

  test.moveAnimal(new Human());
  test.moveAnimal(new Tiger());
  test.moveAnimal(new Eagle());
}

public void moveAnimal(Animal animal){
  animal.move();  // 코드 한 줄로 여러 타입의 메서드 호출이 가능하다.
}
```
하나의 클래스를 상속 받은 여러 클래스가 있는 경우 각 클래스마다 메서드를 오버라이딩 할 수 있다. 상위 클래스 타입으로 선언된 하나의 변수가 여러 인스턴스에 대입되어 다양한 구현을 할 수 있다.

__상속을 언제 사용할까?__  
상속을 사용하지 않고 하나의 클래스에 여러 특성을 한꺼번에 구현하는 경우 코드에 많은 if 문이 생길 수 있다.

__예시__
```java
if(customerGrade == "VIP"){
  ... //할인, 큰 보너스 적립 
}
else if(customerGrade == "Gold"){
  ... //약간의 보너스 적립 
} 
else if(customerGrade == "SILVER"){
  ... // 일반 보너스 적립
}
```
상속을 사용하면 새로운 등급이 생기더라도 Customer 클래스로부터 상속받아 필요한 변수를 추가하고 메서드를 오버라이딩해서 간단히 추가할 수 있다.

- __IS-A 관계__ (is a relationship: inheritance):  
  일반적인(General) 개념과 구체적인(Specific) 개념과의 관계로 단순히 코드를 재사용하는 목적으로 사용하지 않는다.
    - 상위 클래스: 일반적인 개념 클래스 (예: 포유류)
    - 하위 클래스: 구체적인 개념 클래스 (예: 사람, 원숭이, 고래 등등)

- __Has-A 관계__ (Composition):
  한 클래스가 다른 클래스를 소유한 관계  
  코드 재사용의 한 방법으로 Student 클래스가 Subject 클래스를 포함하는 관계
```java
class Student{
  Subject math;
  Subject korean;
}
```

---

## 다운 캐스팅 - instance of
> 하위 클래스가 상위 클래스로 형 변환되는 것을 의미한다. 다시 원래 자료형인 하위 클래스로 형 변환 하려면 명시적으로 다운 캐스팅을 해야 한다. 이 떄 원래 인스턴스의 타입을 체크하는 예약어가 `instanceof`이다.

__예시__
```java
public void moveAnimal(Animal animal){
  animal.move();  // 문제없이 동작함
  
  if(animal instanceof Human){
    Human human = (Human)animal;  // Human 다운 캐스팅이 필요
    human.reading();
  }
  else if(animal instanceof Tiger){ // Tiger 다운 캐스팅이 필요
    Tiger tiger = (Tiger)animal;
    tiger.hunting();
  }
  else if(animal instanceof Eagle){ // Eagle 다운 캐스팅이 필요
    Eagle eagle = (Eagle)animal;
    eagle.flying();
  }

```

끝!
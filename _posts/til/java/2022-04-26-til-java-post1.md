---
layout: post
title: 클래스와 객체
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

## 클래스, 인스턴스

## 클래스란?

>객체에 대한 속성과 기능을 코드로 구현한 것<br>
>객체를 만들기 위한 틀, 붕어빵을 만드는 틀로 비유

- __Property:__ 객체의 Property(속성)은 클래스 내의 변수로 객체의 상태를 말한다.<br>
ex) 학번, 이름, 학년 등

- __Method:__ 함수의 일종으로 객체의 기능을 제공하기 위해서 클래스 내부에 구현되는 함수<br>
ex) 수강신청, 수업듣기, 시험보기 등

__클래스 선언__

```java
접근제어자 class 클래스이름{
  int property;
  void method(){
    원하는 기능;
  }
}
```
---

## 인스턴스
>클래스로부터 객체를 만드는 과정을 인스턴스화라고 하며
>이때 클래스로부터 만들어진 객체를 __인스턴스(Instance)__ 라고 한다.<br>
>붕어빵틀로 구워진 붕어빵에 비유

__인스턴스 선언__
```java
클래스형 변수 이름 = new 생성자;
Student studentA = new Student();
```
하나의 클래스 코드로부터 여러개의 인스턴스를 생성할 수 있는데 인스턴스는 힙 메모리에 생성된다.
<br>

---

## 생성자
>인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드<br>
>인스턴스 생성시 new 키워드와 함께 사용된다.

생성자 내에는 객체가 생성될 때 수행하는 명령어 코드를 넣어주며 생성자는 따로 정의해주지 않으면 자바 컴파일러가 <span style='background-color: #f5f0ff'>Default 생성자</span>를 자동으로 생성해준다.<br>
<mark>하지만 생성자가 하나라도 선언되면 생성자는 생성되지 않는다.</mark>


__Default 생성자__
```JAVA
public Student(){ } // 매개변수, 구현코드가 없음
```

__생성자 오버로드__

```java
public Student(int id, String name){
	studentID = id;
	studentName = name;
}
```

생성자 이름은 클래스 이름과 같고 메서드가 아니기 때문에 상속되지 않고 리턴 값이 없음
<br>

---

## 변수의 자료형
- 기본 자료<br>
ex) int, long, float, double 등
- 참조 자료형<br>
ex) String, Date, Student(클래스 변수) 등<br>

Student 같이 사용자가 만든 클래스 변수가 참조 자료형에 해당되고 주소값을 저장한다.
<br>

---

## 정보 은닉(Information hiding)
객체지향의 특성 중 하나로 외부에서 클래스 내부의 변수나 메서드를 접근하지 못하도록 하는 것을 말한다.<br>
`private` 접근 제어자를 사용을 해서 외부 접근을 불가능하게 하며 `private`으로 선언된 변수에 접근을 할 때는 `get()`, `set()` 메서드를 사용해서 접근한다.

끝!

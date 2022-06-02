---
layout: post
title: 클래스와 객체2
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---
# this, static, Singleton 패턴

* toc
{:toc .large-only}

## this 키워드
>인스턴스 자기 자신을 가리키는 키워드이며 3가지 방법으로 사용된다.
>1. 자기 자신의 메모리를 가리킴
>2. 생성자에서 다른 생성자를 호출
>3. 자기 자신의 주소 반환

__예시 1. 자기 자신의 메모리를 가리킴__

``` java
BirthDay day = new BirthDay();		// BirthDay 클래스 생성
day.setYear(2000);	//day는 BirthDay의 주소를 가리킴
this.setYear(2000);	//this와 day 동일한 역할
```

```java
public class BirthDay{
	int day;
	int month;
	int year;

	public void setYear(int year){	//변수명이 동일할 때 사용됨
		this.year = year;	//BirthdayDay.year = year;
	}
}
```
<mark>컴파일러는 가장 가까운 변수명을 참조하기 떄문에 this를 생략하게 되면 year는 파라미터로 사용되는 year로 인식된다.</mark>

__예시 2. 생성자에서 다른 생성자를 호출__

```java
public class Person{
	String name;
	int age;

	public Person(){	//아래에 이미 정의되어있는 생성자를 쓸 수 있음
		this("이름없음", 0);	//생성자가 여러개면 파라미터 개수와 타입에 따라 자동 매핑
	}
	public Person(String name, int age){
		this.name = name;
		this.age = age;
	}
}
```

__예시 3. 자기 자신의 주소 반환__
```java
public class Person{
	String name;
	int age;

	public Person(String name, int age){
		this.name = name;
		this.age = age;
	}
	public Person getPersonInstance(){
		return this;	// 자기 자신의 참조값 전달
	}
}
```

---

## static 변수

>여러 개의 인스턴스가 같은 메모리 값을 공유하기 위해 사용하며 프로그램이 시작될 때부터 끝날 떄까지 존재한다. 인스턴스에 상관없이 클래스에서 공용되기 때문에 클래스 변수라고 하며 멤버변수는 인스턴스 변수라고 한다.

__static 변수 선언__
```java
static예약어 자료형 변수이름
static int num;
```

__예시__
```java
public class Student{
	static serialNum = 1000;
	int studentID;

	public Student(){
		serialNum++;
		studentID = serialNum;
	}
}

public static void main(String[] args){
	Student hong = new Student();
	Student park = new Student();

	System.out.println(hong.serialNum);	//1001 출력
	System.out.println(park.serialNum);	//1002 출력
	//위에 코드들은 Warning이 생긴다. static은 클래스 변수인데 Instance에서 사용했기 때문
	System.out.println(Student.serialNum);	//그래서 이렇게 클래스명으로 사용 가능
}
```

__static 메서드__<br>
> __static 메서드__
는 클래스 메서드라고도 한다. 메서드에 static 키워드 사용하며 주로 `static` 변수를 위한 기능 제공한다.__static 메서드__
도 인스턴스의 생성과 관계 없이 클래스 이름으로 직접 메서드 호출이 가능하다.


__예시__<br>
<span style='background-color: #f5f0ff'>인스턴스 변수</span>는 인스턴스가 생성되고나서 사용 가능할 수 있기 때문에 <span style='background-color: #f5f0ff'>static 변수</span>에서 <span style='background-color: #f5f0ff'>인스턴스 변수</span>는 사용 불가능

```java
public class Student{
	static serialNum = 1000;
	int studentID;

	public Student(){
		serialNum++;
		studentID = serialNum;
	}
	public static int getSerialNum(){	//static 메서드에서 인스턴스 변수 사용 불가능
		int i = 10;	//지역변수 - stack 영역에 메서드 호출시 생성되어 종료시 소멸
		i++;
		System.out.println(i);

		/* 인스턴스 new 키워드 사용될 때 생성됨
		   인스턴스 생성 전에 Student.getSerialNum() 호출한다고 가정하면
		   생성되지도 않은 변수를 사용하는 것이기 때문에 오류 발생 */
		studentName = "홍길동";	// 멤버변수, 인스턴스 변수

		return serialNum;	// static 변수 or 클래스 변수
	}
}
```

---

## 변수 유효 범위

| 변수 유형 | 선언 위치 | 사용 범위 | 메모리 | 생성과 소멸 |
| ---------- | ------ | ------ | ----- | ----- |
| __지역 변수__ <br>(로컬 변수) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 함수 내부 | 함수 내부| 스택 &nbsp;&nbsp;&nbsp;&nbsp; | 함수 호출시 생성<br> 함수 종료시 소멸|
| __멤버 변수__ <br> (인스턴스 변수)| 클래스 멤버 변수로 선언 | 클래스 내부<br> private이 아니라면 참조 변수로 다른 클래스에서 사용 가능| 힙 | 인스턴스 생성시 힙에 생성<br> 가비지 컬렉터가 메모리 수거할 때 소멸&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| __static 변수__<br> (클래스 변수) | static 예약어를 사용해 클래스 내부 | 클래스 내부<br> private이 아니면 클래스 이름으로 다른 클래스에서 사용 가능| 데이터 영역 | 프로그램 시작시 생성<br> 프로그램 종료시 소멸 |

---

## Singleton 패턴

>디자인 패턴 중 하나로 전 시스템에 단 하나의 인스턴스만이 존재하도록 구현하는 방식
>- static 변수 사용
>- 생성자를 private으로 생성
>- public으로 선언된 static 메서드 제공

> __디자인 패턴__ 이란? 객체지향에서 자주 사용되는 설계 방법으로 소프트웨어를 설계하면서 자주 발생하는 문제들을 해결하기 위한 방법들을 일반화하여 정리한 것 

__예시__

```java
public class Company{

	private static Company instance = new Company();	// 유일하게 사용될 인스턴스
	//private으로 선언되어 함부로 인스턴스 생성 불가능
	private Company(){}	//여러 인스턴스 생성을 방지하기 위해 생성자 직접 정의

	//외부에서 가져다 쓰기 위한 public method
	public static Company getInstance(){	//return 타입 Comapny
		if(instance == null)	instance = new Company();	//예외 처리
		return instance;
	}
}

public static void main(String[] args){	//외부 main문
	Company c1 = company.getInstance();
	Company c2 = company.getInstance();

	System.out.println(c1 == c2); // true 출력, 여러 인스턴스 생성 불가능
}
```
라이브러리에 `Calendar` 같은 클래스가 singleton으로 설계된다.

끝!
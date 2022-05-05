---
layout: post
title: 기본 클래스
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

## Java 기본 클래스

* toc
{:toc .large-only}

## java.lang 패키지
- 프로그래밍시 `import` 하지 않아도 자동으로 `import`
- 자동으로 `import java.lang.*;` 문장이 추가 된다.
- 주로 사용되는 기본 클래스들이 속해있다.
- ex) String, Integer, System 등

---

## Object 클래스
- 모든 클래스의 취상위 클래스
- java.lang.Object 클래스
- 모든 클래스는 Object 클래스에서 상속 받는다.
- 모든 클래스는 Object 클래스의 메서드를 사용할 수 있다.
- 모든 클래스는 Object 클래스의 메서드 중 `final`로 선언된 메서드 외 일부는 재정의 할 수 있다.
- 컴파일러는 모든 클래스에 자동으로 'extends Object'를 추가한다.
```java
public class Student extends Object{
  ...
}
```

## Object 클래스 메서드

| 메서드 | 설명 |
| ------ | -------- |
| String __toString()__ | 객체를 문자열로 표현하여 반환. 재정의하여 객체에 대한 설명이나 특정 멤버 변수 값 반환 |
| boolean __equals(__ Object obj __)__ | 두 인스턴스가 동일한지 여부 반환. 재정의하여 논리적으로 동일한 인스턴스임을 정의할 수 있음 |
| int __hashCode()__ | 객체의 해시 코드 값 반환 | 
| Object __clone()__ | 객체를 복제하여 동일한 멤버 변수 값을 가진 새로운 인스턴스 생성 |
|Class __getClass()__ | 객체의 Class 클래스를 반환 |
|void __finalize()__ | 인스턴스가 힙 메모리에서 제거될 때 가비지 컬렉터(GC)에 의해 호출되는 메서드. 네트워크 연결 해제, 열려 있는 파일 스트링 해제 등을 구현 |
|void __wait()__ | 멀티스레드 프로그램에서 사용하는 메서드. 스레드를 '기다리는 상태'(non runnable)로 만듦 |
|void __notify()__ | wait() 메서드에 의해 기다리고 있는 스레드(nonrunnable 상태)를 실행 가능한 상태(runnable)로 가져옴 |

__toString 메서드__
- 객체의 정보를 String으로 바꾸어 사용
- String이나 Integer 클래스에는 이미 재정의 되어 있음
- String은 문자열 반환
- Integer는 정수 값 반환

```java
class Book{
  
  ...

	@Override
	public String toString() {  //toString 재정의
		return title + ", " + author;
	}
}
```

```java
public class ToStringEx {
	public static void main(String[] args) {
		Book book = new Book("두잇자바", "박은종");
		System.out.println(book); //주소값이 아니라 "두잇자바, 박은종" 출력
	}
}
```

__equals() 메서드__
- 두 인스턴스의 주소 값을 비교하여 `true`/`false` 반환
- 재정의하여 두 인트섵스가 논리적으로 동일함 여부 반환

``` java
class Student{

...

	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Student) {
			Student std = (Student)obj;
			if(studentID == std.studentID) 
				return true;
			else 
				return false;
		}
		return false;
	}
}
```

```java
public class EqualsTest {
	public static void main(String[] args) {
	
		Student std1 = new Student(10001, "Tomas");
		Student std2 = new Student(10001, "Tomas");
				
		System.out.println(std1 == std2); //주소가 다르기 때문에 "false" 출력
		System.out.println(std1.equals(std2));  //논리적으로 동일 "true" 출력
				
	}
}
```

__hashCode 메서드__
- hash: 정보를 저장, 검색하기 위해 사용하는 자료구조
- 자료의 특정 값(키 값)에 대한 저장 위치를 반환해주는 해시 함수를 사용
```java
index = hash(key);
```
- 해시 함수는 어떤 정보인가에 따라 다르게 구현 됨
- `hashCode()` 메서드는 인스턴스의 저장 주소를 반환
- 힙 메모리에 인스턴스가 저장되는 방식이 __hash__

논리적으로 동일함을 위해 `equals()` 메서드를 재정의 했다면 `hashCode()` 메서드를 재정의하여 동일한 값이 반환되도록 한다. <mark>equals() 메서드를 재정의했지만 hashCode() 메서드를 재정의 하지 않는다면 다른 클래스에서 사용시에 오류가 생길 수 있다</mark>

```java
@Override
public int hashCode() {
	return studentID;
}
```

__clone() 메서드__
- 객체의 원본 복제하는데 사용하는 메서드
- 원본을 유지해 놓고 복사본을 사용할 떄
- 기본 틀(prototype)을 두고 복잡한 생성과정을 반복하지 않고 복제
- `clone()` 메서드를 사용하면 객체의 정보(멤버 변수 값)가 같은 인스턴스가 또 생성되는 것이기 때문에 객체 지향 프로그램의 정보은닉, 객체 보호 관점에서 위배될 수 있다.
- 객체의 `clone()` 메서드 사용을 허용한다는 의미로 `cloneable` 인터페이스를 명시해 줌

```java
class Circle implements Cloneable{  // 객체를 복제해도 된다는 의미의 'Cloneable' 인터페이스
  Point point;
  int radius;
}
```
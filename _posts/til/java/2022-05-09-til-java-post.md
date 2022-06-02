---
layout: post
title: 내부 클래스
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

# 람다식(lambda expression)

* toc
{:toc .large-only}

## 람다식이란?
>자바에서 <span style='background-color: #f5f0ff'>함수형 프로그래밍(functional programming)</span>을 구현하는 방식을 말한다. Java 8부터 지원을 하며 클래스를 생성하지 않고 함수의 호출만으로 기능을 수행하는 방식이다.

__함수형 프로그래밍이란?__
>순수 함수(pure function)를 구현하고 호출함으로써 외부 자료에 부수적인 영향을 주지 않고 매개 변수만을 사용하도록 만든 함수이다.
- 함수를 기반으로 구현
- 입력 받은 자료를 기반으로 수행되고 외부에 영향을 미치지 않으므로 병렬 처리 등에 가능
- 안정적인 확장성 있는 프로그래밍 방식

---

## 람다식 구현

매개 변수와 매개 변수를 활용한 실행문으로 구현

__구현 방법__
```java
(매개 변수) -> {실행문;}
```
__일반 함수 예시__
```java
int add(int x, int y){
  return x + y;
}
```

__람다식 예시__
```java
(int x, int y) -> {return x + y;}
```
함수 이름 반환 형을 없애고 `->`를 사용

---

## 람다식 문법

- 매개변수가 하나인 경우 자료형과 괄호 생략 가능

```java
str -> {System.our.println(str);}
```
- 매개 변수가 두 개인 경우 괄호 <mark>생략 불가능</mark>
```java
x, y -> {System.our.println(x + y);}  //잘못된 표현
```
- 중괄호 안의 구현부가 한 문장인 경우 중괄호 생략 가능
```java
str -> System.our.println(str);
```
- 중괄호 안의 구현부가 한 문장이라도 `return`문은 중괄호 <mark>생략 불가능</mark>
```java
str -> return System.our.println(); //잘못된 표현
```
- 중괄호 안의 구현부가 반환문 하나라면 `return`과 중괄호 모두 생략 가능
```java
(x, y) -> x + y
str -> str.length()
```

---

## 람다식 사용

__함수형 인터페이스 선언__

```java
@FunctionalInterface
public interface MyNumber { //하나의 메서드만 가져야 함
	int getMaxNumber(int num1, int num2);
}
```
람다식을 선언하기 위해 인터페이스를 구현해야 한다. 익명 함수와 매개 변수만으로 구현되므로 <mark>두 개 이상의 메서드인 경우 어떤 메서드의 호출인지 모호해 진다.</mark> `@FunctionalInterface` 애노테이션을 표시하여 여러 개의 메서드를 선언하면 에러가 나도록 구현한다.

__람다식 구현 및 호출__
```java
public class TestMyNumber {
	public static void main(String[] args) {
		MyNumber maxNum = (x, y) -> (x >= y) ? x : y; //람다식 인터페이스형 변수에 대입

		System.out.println(maxNum.getMaxNumber(10, 20));  //인터페이스형 변수로 메서드 호출
	}
}
```

자바는 객체 지향 언어이기 때문에 객체를 생성해야 메서드가 호출이 된다. 람다식으로 메서드를 구현하고 호출하면 내부에서 <span style='background-color: #f5f0ff'>익명 클래스</span>가 생성된다.

익명 내부 클래스 구현과 동일
```java
MyNumber maxNum = new MyNumber() {		

  @Override
  public int getMaxNumber(int num1, int num2) {
    return (num1 >= num2) ? num1 : num2;
  }
};
```

끝!
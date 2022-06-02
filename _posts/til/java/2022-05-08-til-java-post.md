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

# 내부 클래스

* toc
{:toc .large-only}

## 내부 클래스 요약

| 종류 | 구현 위치 | 사용할 수 있는 외부 클래스 변수 | 생성 방법 |
| --- | --- | --- | --- |
| 인스턴스 내부 클래스 | 외부 클래스 멤버 변수와 동일 | 외부 인스턴스 변수 외부 전역 변수 | 외부 클래스를 먼저 만든 후 내부 클래스 생성|
| 정적 내부 클래스 | 외부 클래스 멤버 변수와 동일 | 외부 전역 변수&nbsp;&nbsp;&nbsp;&nbsp; | 외부 클래스와 무관하게 생성 |
| 지역 내부 클래스 | 메서드 내부에 구현 | 외부 인스턴스 변수 외부 전역 변수 | 메서드를 호출할 때 생성 |
| 익명 내부 클래스 | 메서드 내부에 구현, 변수에 대입하여 직접 구현 | 외부 인스턴스 변수 외부 전역 변수 | 메서드를 호출할 때 생성되거나, 인터페이스 타입 변수에 대입할 때 `new` 예약어를 사용하여 생성 |

__인스턴스 내부 클래스__

```java
class OutClass{
	private int num = 10;
	private static int sNum = 20;
	private InClass inClass;
	
	public OutClass(){
		inClass = new InClass();
	}
	
	private class InClass{  //주로 private으로 선언
		int inNum = 200;
		static int sInNum = 100;
		
		void inTest() {
			System.out.println(num);
			System.out.println(sNum);
		}
		static void sTest() {

		}
	}
	
	public void usingInTest() { //private 클래스이기 때문에 메서드를 사용
		inClass.inTest();
	}
}
```
__main 문__
```java
OutClass outClass = new OutClass();
outClass.usingInTest();
```

---

__정적 내부 클래스__

```java
class OutClass{
	private int num = 10;
	private static int sNum = 20;
	private InClass inClass;
	
	...
	
	static class InStaticClass{
		int iNum = 100;
		static int sInNum = 200;
		
		void inTest() {
			sNum += 10;
			System.out.println(sNum);
			System.out.println(iNum);
			System.out.println(sInNum);
		}
		static void sTest() {
			System.out.println(sNum);
      //System.out.println(iNum); // staic 메서드에서 멤버 변수 호출 불가능 
			System.out.println(sInNum);
			
		}
	}
}
```
__main 문__
```java
OutClass.InStaticClass sInClass = new OutClass.InStaticClass();
sInClass.inTest();
OutClass.InStaticClass.sTest();
```

---

__지역 내부 클래스__
```java
class Outer{
	
	int outNum = 100;
	static int sNum = 100;

	
	public Runnable getRunable(final int i){
		
		final int localNum = 100;
		class MyRunnable implements Runnable{

			@Override
			public void run() {
				System.out.println(outNum);
				System.out.println(sNum);
				System.out.println(localNum);
				System.out.println(i);
			}
		}
		return new MyRunnable();
	}
}

```
__main 문__
```java
Outer outer = new Outer();		
Runnable runnable = outer.getRunable(20);
runnable.run();
```

---

__익명 내부 클래스__
```java
class Outer{
int outNum = 100;
static int sNum = 100;
	
	Runnable runnable = new Runnable() {
		
		@Override
		public void run() {
			System.out.println(outNum);
			System.out.println(sNum);
		}
	};
}
```
__main 문__
```java
outer.runnable.run();
```

끝!
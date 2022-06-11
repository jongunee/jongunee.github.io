---
layout: post
title: 제네릭(Generic) 프로그래밍
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - java
---

# 제네릭(Generic) 프로그래밍

* toc
{:toc .large-only}

## 제네릭 프로그래밍이란?
> 변수의 선언이나 메서드의 매개변수를 하나의 참조 자료형이 아닌  <span style='background-color: #f5f0ff'>여러 자료형</span>을 변환 될 수 있도록 프로그래밍 하는 방식. 실제 사용되는 참조 자료형으로의 변환은 컴파일러가 검증하므로 안정적인 프로그래밍 방식이며  <span style='background-color: #f5f0ff'>컬렉션 프레임워크</span>에서 많이 사용되고 있다.

## 제네릭 클래스란?
>여러 참조 자료형으로 대체 될 수 있는 부분을 하나의 문자로 표현하며 이 문자를 자료형 매개변수라고 한다.

__예시__

```java
public class ThreeDPrinter<T extends Material> {  //상속받은 것만으로 제한 할 수도 있음
	private T material;

	public T getMaterial() {
		return material;
	}

	public void setMaterial(T material) {
		this.material = material;
	}
}
```

__자료형 매개 변수 T__
- type의 의미로 T를 많이 사용 함
- `<T>`에서 `<>`는 다이아몬드 연산자라고 함
- `static` 키워드는 `T`에 사용할 수 없음
- 다이아몬드 연산자 내부에서 자료형은 생략 가능  
```java
ArrayList<String> list = new ArrayList<>();
```
- 제네릭에서 자료형 추론 (자바10부터 가능)
```java
ArrayList<String> list = new ArrayList<String>();
➡️ var list = new ArrayList<String>();
```

__제네릭 클래스 사용__
```java
ThreeDPrinter<Powder> printer = new ThreeDPrinter<Powder>();
printer.setMaterial(new Powder());
Powder powder = printer.getMaterial();  //명시적 형 변환 필요 없음
System.out.println(printer);
```

| 용어 | 설명 |
| --- | --- |
| GenericPrinter<Posder> | 제네릭 자료형(Generic type), 매개변수화된 자료형(parameterized type) |
| Powder | 대입된 자료형 |

__T extends 클래스__  
T가 사용될 클래스를 제한하기 위해 사용
```java
public class Plastic extends Material{
	...
}
```

```java
public class Powder extends Material{
	...
}
```

```java
public class Water{
	...
}
```

```java
public class ThreeDPrinter<T extends Material> { //상속받은 것만으로 제한한다
	...
}
```
<mark>Material을 상속받은 Plastic, Powder만 사용이 가능하고 Water는 사용이 불가능하다.</mark>

---

## 제네릭 메서드
>메서드의 매개변수를 자료형 매개변수로 사용하는 경우 또는 자료형 매개 변수가 하나 이상인 경우

__제네렉 메서드의 일반 형식__

```java
public <자료형 매개변수> 반환형 메서드 이름(자료형 매개변수 ...) {}
```

__자료형 매개 변수가 두 개인 경우__
```java
public class Point<T, V>{
	T x;
	V y;

	Point(T x, V y){
		this.x = x;
		this.y = y;
	}
	
	public T getX(){	//제너릭 메서드
		return x;
	}
	
	public T getY(){	//제너릭 메서드
		return y;
	}
}
```

__제네릭 메서드 구현__  

__사각형 너비 구하기__
```java
public class GenericMethod{
	//제네릭 메서드
	public static<T, v> double makeRectangle(Point<T, V> p1, Point<T, V> p2){
		double left = ((Number)p1.getX()).doubleValue();
		double right = ((Number)p2.getX()).doubleValue();
		double top = ((Number)p3.getY()).doubleValue();
		double bottom = ((Number)p4.getY()).doubleValue();
		
		double width = right - left;
		double height = bottom - top;

		return width * height;
	}
}
```
__Main문__
```java
Point<Integer, Double> p1 = newe Point<Integer, Double>(0, 0.0);
Point<Integer, Double> p2 = newe Point<>(10, 10.0);

double rect = GenericMethod.<Integer, Double>makeRectangle(p1, p2);
```

끝!
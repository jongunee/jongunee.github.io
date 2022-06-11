---
layout: post
title: 배열과 ArrayList
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - java
---
# 배열, ArrayList

* toc
{:toc .large-only}

## 배열
> 동일한 자료형의 변수를 한꺼번에 순차적으로 관리하기 위한 데이터 타입. 
> 연속적인 변수를 하나씩 관리하려면 비효율적이고 관리하기 어렵기 떄문에 사용된다.

__배열 선언__
```java
자료형[] 배열이름 = new 자료형[개수];
int[] arr = new int[10];

자료형 배열이름[] = new 자료형[개수];
int arr[] = new int[10];
```

__배열 초기화__

숫자를 넣어주지 않으면 int형은 0, double형은 0.0으로 초기화된다.

```java
int[] numbers = new int[] {1, 2, 3};
```
```java
int[] numbers = new int[3];

numbers[0] = 1;
numbers[1] = 2;
numbers[2] = 3;
```
```java
int[] numbers;
numbers = new int[3];

numbers[0] = 1;
numbers[1] = 2;
numbers[2] = 3;
```
```java
int[] numbers = {1, 2, 3};
```

---

## 객체 배열

객체 배열 각 요소는 new를 활용하여 생성해서 저장해야 하고 값은 null로 초기화된다.
<br><br>
__객체 배열 선언__

```java
Book[] library = new Book[3];

Library[0] = new Book("태백산맥", "조경래");
Library[1] = new Book("토지", "박경리");
Library[2] = new Book("어린왕자", "생택쥐페리");
```

---

## 배열 복사

기존 배열과 같은 배열을 만들거나 배열이 꽉 찬 경우 더 큰 배열을 만들어 복사할 수 있다.
```java
System.arraycopy(src, srcPos, dest, destPos, length);
```

| 매개변수 | 설명 |
| ------ | -------- |
| __src__ | 복사할 배열 이름 |
| __srcPos__ | 복사할 배열의 첫 번쨰 위치 |
| __dest__ | 붙여 넣을 대상 배열 이름 | 
| __destPos__ | 붙여 넣기를 시작할 대상 배열의 첫 번째 위치 |
|__length__ | src에서 dest로 복사할 요소 개수 |

__예시__

```java
int[] array1 = {10, 20, 30, 40, 50};
int[] array2 = {1, 2, 3, 4, 5;

system.arraycopy(array1, 0, array2, 1, 4); // array2는 {1, 10, 20, 30, 40}
```

__객체 배열 복사__

일반 배열과 동일하게 `arraycopy` 메서드를 사용할 수 있다.
```java
Book[] library1 = new Book[3];
Book[] library2 = new Book[3];

library1[0] = new Book("태백산맥", "조경래");
library1[1] = new Book("토지", "박경리");
library1[2] = new Book("어린왕자", "생택쥐페리");

System.arraycopy(library1, 0, library2, 0, 3);	//library2에 library1 주소 복사

//주소가 복사되기 때문에 library1[0].BookName, library2[1].BookName 모두 바뀜
library1[0].setBookName("나목");
```
<mark>하지만, 값이 아니라 주소가 복사되기 때문에 복사 이후에 수정하면 값이 같이 수정된다.</mark><br>
같이 수정되는 것을 원하지 않는다면 따로 객체를 생성해서 복사해 줘야한다.
```java
Book[] library2 = new Book[3];

library2[0] = new Book();
library2[1] = new Book();
library2[2] = new Book();

for(int i = 0; i < library1.length; i++){
	library2[i].setBookName(library1[i].getBookName);
}
```

---

## 향상된 for문 (Enhanced for loop)

>배열 요소의 처음부터 끝까지 모든 요소를 참조할 때 편리한 반복문

__예시__

```java
String[] strArr = {"Java", "Android", "C"};

for(String s : strArr){
	System.out.println(s);
}
```

---

## 다차원 배열

2차원 이상의 배열로 지도, 게임 등 평면이나 공간을 구현할 때 많이 사용된다.

__다차원 배열 선언__
```java
자료형[][] 배열 이름 = new 자료형 [행 개수][열 개수];
int[][] arr = new int[2][3];
```
__다차원 배열 초기화__

```java
int[][] arr = { {1, 2, 3}, {4, 5, 6} };
```

---

## ArrayList

>ArrayList는 JDK에서 제공하는 클래스이다. <br>
>- 기존 배열은 길이를 정해서 선언해야하기 때문에 사용 도중 부족한 경우 다른 배열로 복사하는 코드를 직접 구현해야 한다. 또한 중간의 요소를 삽입, 삭제 하는 경우에 대한 코드도 구현해야 한다. <br>
>- 하지만, ArrayList 클래스는 자바에서 제공되는 객체 배열이 구현된 클래스로 여러 메서드와 속성 등을 사용해서 편리하게 관리할 수 있다.

__ArrayList 선언__
```java
import java.util.ArrayList; // 라이브러리

ArrayList<String> list = new ArrayList<String>(); //ArrayList 선언
```
`Ctrl` + `Shift` + `o`를 입력해서 필요한 라이브러리 import 할 수 있다. 

__ArrayList 클래스 주요 메서드__

| 메서드 | 설명 |
| ------ | -------- |
| boolean __add(E e)__ | 요소 하나를 배열에 추가. E는 요소의 자료형을 의미 |
| int __size()__ | 배열에 추가된 요소 전체 개수 반환 |
| E __get(int index)__ | 배열의 index 위치에 있는 요소 값 반환 | 
| E __remove(int index)__ | 배열의 index 위치에 있는 요소 값을 제거하고 그 값 반환 |
|boolean __isEmpty()__ | 배열이 비어 있는지 확인 |

요소를 추가/제거하기가 편리하다.

__예시__
```java
ArrayList<String> list = new ArrayList<String>();
list.add("aaa");
list.add("bbb");
list.add("ccc");
```

끝!
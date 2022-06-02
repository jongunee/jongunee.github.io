---
layout: post
title: 컬렉션 프레임워크
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

# Collection, List, Set, Map

* toc
{:toc .large-only}

## 컬렉션 프레임워크란?
> 프로그램 구현에 필요한 자료구조를 구현해 놓은 라이브러리로 `java.util` 패키지에 구현되어 있다. 개발에 소요된는 시간을 절약하면서 최적화 알고리즘을 사용할 수 있다.

![그림1](/assets/img/etc/data%20structure.png)
- `Collection`과 `Map`은 `Interface` - 구현 코드가 없음
- `Collection` 계열은 하나의 데이터만 Handling
- `Map` 계열은 쌍으로 된 데이터 Handling
- `List`는 선형 자료 구조 - 한 줄로 열거된 순서가 있는 형태
- `set`은 중복된 요소가 허용되지 않는 구조 - 순서와 무관하며 ID, 학번 등 관리에 사용
- `TreeSet`은 정렬에 주로 사용
- `HashTable`은 동기화 지원 `HashMap` 동기화 지원 X
- `TreeMap` 키 값으로 정렬
- `Properties` - 파일에서 읽어서 Key-Value 쌍으로 데이터 저장 가능
- `Iterator`는 순회한다는 의미

---

### Collection 인터페이스
> <span style='background-color: #f5f0ff'>하나의 객체</span>를 관리하기 위한 메서드가 정의된 인터페이스로 하위에 `List`, `Set` 인터페이스가 있다.

__Collection 인터페이스 주요 메서드__

| 메서드 | 설명 |
| --- | --- |
| boolean __add(__ E e __)__ | Collection에 객체를 추가 |
| void __clear()__ | Collection의 모든 객체 제거 |
| Iterator\<E> __iterator__ | Collection을 순환할 반복자(Iterator)를 반환 |
| boolean __remove(__ Objet o __)__ | Coolection에 매개변수에 해당하는 인스턴스가 존재하면 제거 |
| int __size()__ | Collection에 있는 요소 개수 반환 |

---

### List 인터페이스
> Collection의 하위 인터페이스로 객체를 <span style='background-color: #f5f0ff'>순서에 따라 저장</span>하고 관리하는데 필요한 메서드가 선언된 인터페이스이다. <span style='background-color: #f5f0ff'>배열의 기능</span>을 구현하기 위한 인터페이스라고 할 수 있다.

---

### ArrayList, Vector
> 객체 배열을 구현한 클래스이다. `Vector`는 자바2부터 제공된 클래스로 보통 멀티 쓰레드 상태에서 리소스에 대한 동기화가 필요한 경우 사용한다. 하지만 단일 쓰레드에서 동기화를 사용하는 것은 오버헤드이기 때문에 일반적으로는 `ArrayList`를 사용한다. 그리고 동기화 기능이 추가 되어야 하는 경우에는 아래 `synchrodizedList` 메서드를 사용된다. 

```java
Collections.synchronizedList(new ArrayList<String>());
```
<span style='background-color: #f5f0ff'>동기화</span>는 두 개의 쓰레드가 동시에 하나의 리소스에 접근 할 떄 순서를 맞추어서 데이터에 오류가 발생하지 않도록 한다.

---

### LinkedList
> 논리적으로 순차적인 자료구조가 구현된 클래스로 각 노드가 <span style='background-color: #f5f0ff'>데이터와 다음 노드의 주소</span>로 구성되어 있다. 데이터 <span style='background-color: #f5f0ff'>추가/삭제</span>에 드는 배용이 배열보다 적다는 장점이 있다.

---

### Stack, Queue
> `Stack`과 `Queue`의 기능이 구현된 클래스가 있지만 `ArrayList`와 `LinkedList`를 활용해서 사용할 수도 있다.

---

### Iterator 사용한 순회
- `Collection`의 개체를 순회하는 인터페이스
- `iterator()` 메서드 호출
```java
Iterator ir = memberArrayList.iterator( );
```
- 선언된 메서드

| 메서드 | 설명 |
| --- | --- |
| boolean __hashNext()__ | 이후에 요소가 더 있는지를 체크하는 메서드이며, 요소가 있다면 `true` 반환 |
| E __next()__ | 다음에 있는 요소를 반환 |

__예시__
```java
Iterator<Member> iterator = arrayList.iterator();
while(iterator.hasNext()) {	//다음 요소가 있나? 없으면 while문 빠져 나옴
  Member member = iterator.next();
  
  int tempId = member.getMemberId();
  if(memberId == tempId) {
    arrayList.remove(member);
    return true;
  }
}
```

---

### Set 인터페이스
> `Collection` 하위 인터페이스이며 중복을 허용하지 않는다. `List`는 순서 기반이지만 `Set`은 순서가 없다는 차이점이 있고 보통 아이디, 주민번호, 사번 등 <span style='background-color: #f5f0ff'>유일한 값</span>이나 <span style='background-color: #f5f0ff'>객체 관리</span>에 사용된다. <mark>순서 기반이 아니기 때문에 get(i) 메서드가 제공되지 않는다.</mark>


__hashCode, equals 재정의__
```java
@Override
public int hashCode() {
  return memberId;
}

@Override
public boolean equals(Object obj) {
  if(obj instanceof Member) {
    Member member = (Member) obj;
    
    if(this.memberId == member.memberId) {
      return true;
    }
    else return false;
  }
  return false;
}
```

__main문 예시__
```java
MemberHashSet memberHashSet = new MemberHashSet();

		Member memberLee = new Member(101, "이순신");
		Member memberKim = new Member(102, "김유신");
		Member memberShin = new Member(103, "신사임당");

		
		memberHashSet.addMember(memberLee);
		memberHashSet.addMember(memberKim);
		memberHashSet.addMember(memberShin);
		
		memberHashSet.showAll();
		Member memberLee2 = new Member(101, "이몽룡");  //HashSet은 중복을 허용하지 않는다.
		memberHashSet.addMember(memberLee2);
		memberHashSet.showAll();
```
해시코드는 중복을 허용하지 않기 때문에 위 예시에서는 오버라이딩을 통해 `memberId`를 `hashCode`로 설정하고 `memberId`가 동일하면 동일인으로 설정하여 중복을 방지해주었다.

---

### TreeSet 클래스

>`TreeSet`은 중복을 허용하지 않으면서 오름차순이나 내림차순으로 객체를 정렬이 가능하기 때문에 정렬에 사용되는 클래스이다. 내부적으로 <span style='background-color: #f5f0ff'>이진 검색 트리(binary search tree)</span>로 구현되어 있으며 이진 검색 트리에 자료가 저장 될 때 비교하여 저장될 위치를 정한다. 객체 비교를 위해서는 `Comparable`이나 `Comparator` 인터페이스를 구현 해야 한다.

__Comparable, Comparator 인터페이스__
- 정렬 대상이 되는 클래스가 구현해야 하는 인터페이스
- `Comparable`은 `compareTo()` 메서드를 구현
  - 매개 변수와 객체 자신(`this`)를 비교
- `Comparator`는 `compare()` 메서드를 구현
  - 두 개의 매개 변수를 비교
- `TreeSet` 생성자에 `Comparator`가 구현된 객체를 매개변수를 전달
```java
TreeSet<Member> treeSet = new TreeSet<Member>(new Member());
```
- 일반적으로 `Comparable`을 더 많이 사용
- 이미 `Comparable`이 구현된 경우 `Comparator`를 이용하여 다른 정렬 방식을 정의할 수 있음


__1. Comparable 구현__
```java
public class Member implements Comparable<Member>{
  ...
}
```
`Comparable` 인터페이스를 구현해주고

```java
@Override
public int compareTo(Member member) {

  return (this.memberId - member.memberId) ;  //양수면 오름차순, 음수면 내림차순
}
```
`compareTo()` 메서드를 재정의하여 `memberId`에 따라 정렬이 된다. 


__2. Comparator 구현__
```java
public class Member implements Comparable<Member>, Comparator<Member>{
  ...
}
```
`Comparator` 인터페이스를 구현해주고

```java
@Override
public int compare(Member member1, Member member2) {
  return member1.memberId - member2.memberId;
}
```
`compare()` 메서드를 재정의하여 `memberId`에 따라 정렬이 된다. 

__예시__  
`TreeSet`에서 String은 오름차순으로 정렬 되어 있는데 내림차순으로 정렬하고 싶다!

```java
class MyCompare implements Comparator<String>{

	@Override
	public int compare(String str1, String str2) {
		return str1.compareTo(str2) * -1;
	}
}
```
`Comparator` 인터페이스를 구현한 클래스를 하나 생성해서 `compare` 메서드를 재정의

```java
public class ComparatorTest {
	public static void main(String[] args) {
		TreeSet<String> tree = new TreeSet<String>(new MyCompare());  //비교를 위한 MyCompare 추가
		
		tree.add("aaa");
		tree.add("ccc");
		tree.add("bbb");
		
		System.out.println(tree); //[ccc, bbb, aaa] 출력
	}
}
```

---

### Map 인터페이스
><span style='background-color: #f5f0ff'>쌍(pair)</span>로 이루어진 객체를 관리하는 데 사용되며 <span style='background-color: #f5f0ff'>key-value의 쌍</span>으로 이루어진다. 여기서 key는 중복될 수 없다.
- key-value pair의 객체를 관리하는데 필요한 메서드가 정의 됨
- <span style='background-color: #f5f0ff'>key</span>는 중복될 수 없음
- 검색을 위한 자료 구조
- <span style='background-color: #f5f0ff'>key</span>를 이용하여 값을 저장하거나 검색, 삭제 할 때 사용하면 편리함
- 내부적으로 <span style='background-color: #f5f0ff'>hash</span> 방식으로 구현 됨

```java
index = hash(key) //index는 저장 위치
```
- key가 되는 객체는 객체의 유일성함의 여부를 알기 위해 `equals()`와 `hashCode()` 메서드를 재정의

### TreeMap 클래스
- Key 객체를 정렬하여 key-value를 pair로 관리하는 클래스
- key에 사용되는 클래스에 Comparable, Comparator 인터페이스를 구현
- java에 많은 클래스들은 이미 Comparable이 구현되어 있음
- 구현 된 클래스를 key로 사용하는 경우는 구현할 필요 없음

```java
public final class Integer extends Number implements Comparable<integer>{
  ...
  public int compareTo(Integer anotherInteger){
    return compare(this.value, anotherInteger.value);
  }
}
```

끝!
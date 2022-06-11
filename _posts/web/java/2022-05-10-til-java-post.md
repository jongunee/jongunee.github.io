---
layout: post
title: 내부 클래스
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - java
---

# 스트림

* toc
{:toc .large-only}

## 스트림(Stream)
- 자료의 대상과 관계없이 동일한 연산을 수행
  - 배열 컬랙션을 대상으로 동일한 연산을 수행
  - 일관성 있는 연산으로 자료의 처리를 쉽고 간단하게 함
- 한 번 생성하고 사용한 스트림은 재사용 불가능
  - 자료에 대한 스트림을 생성하여 연산을 수행하면 스트림은 소모됨
  - 다른 연산을 위해서는 새로운 스트림을 생성 함
- 스트림 연산은 기존 자료를 변경하지 않음
  - 자료에 대한 스트림을 생성하면 별도의 메모리 공간을 사용
  - 따라서 기존 자료를 변경하지 않음
- 스트림 연산은 중간 연산과 최종 연산으로 구분 됨
  - 스트림에 대한 중간 연산은 여러 개 적용될 수 있지만 최종 연산은 마지막에 한 번만 적용됨
  - 최종 연산이 호출되어야 중간 연산의 결과가 모두 적용됨
  - 이를 <span style='background-color: #f5f0ff'>지연 연산</span>이라 함



__스트림 연산 - 중간 연산__
- 중간 연산 - `filter()`, `map()`  
  중간에 맞는 요소를 추출(`filter()`)하거나 요소를 변환 함(`map()`)
- 문자열의 길이가 5 이상인 요소만 출력하기

```java
sList.stream().filter(s -> s.length() >= 5).forEach(s -> System.out.println(s));
```

- 고객 클래스에서 고객 이름만 가져오기

```java
customerList.stream().map(c -> c.getName()).forEach(s -> System.out.println(s));
```

- 배열 숫자 총합 구하기

```java
int[] arr = {1, 2, 3, 4, 5};
System.out.println(Arrays.stream(arr).sum());
```

- 배열 Sorting 후에 ArrayList 출력하기

```java
List<String> sList = new ArrayList<String>();
sList.add("Tomas");
sList.add("James");
sList.add("Edward");

sList.stream().sorted().forEach(s -> System.out.println(s));
```

---

## reduce() 연산

- 정의된 연산이 아닌 프로그래머가 직접 지정하는 연산을 적용
- 최종 연산으로 스트림의 요소를 소모하며 연산 수행
- 모든 스트림 요소를 처리해서 값으로 도출
- 배열의 모든 요소의 합을 구하는 `reduce()` 연산

```java
Arrays.stream(arr).reduce(0, (a, b) -> a + b);  //0은 초기값
```

- 두 번째 요소로 전달되는 람다식에 따라 다양한 기능을 수행
- 배열의 가장 긴 문자열 출력

```java
String[] greetings = {"안녕하세요~~~", "Hello", "Good Morning", "반갑습니다"};
		
System.out.println(Arrays.stream(greetings).reduce("", (s1, s2) -> {
  if(s1.getBytes().length >= s2.getBytes().length)
    return s1;
  else	
    return s2;
  }
));
```

또는

```java
class CompareString implements BinaryOperator<String>{
	@Override
	public String apply(String s1, String s2) {
		if(s1.getBytes().length >= s2.getBytes().length)
			return s1;
		else	
      return s2;
	}
}
```
```java
String str = Arrays.stream(greetings).reduce(new CompareString()).get();
System.out.println(str);
```

끝!
---
layout: post
title: String \#9. 숫자만 추출
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#9. 숫자만 추출

* toc
{:toc .large-only}

## 문제: 

> 문자열에서 숫자만 추추라여 자연수를 만듦  
> ※ 자연수이기 때문에  시작 숫자가 '0'이면 안됨

## 내 코드 #1:

```java
public class Problem9 {
  public String solution(String str){
	String ans = "";
	
	StringBuilder num_only = new StringBuilder();
	
	for(char c : str.toCharArray()) {
		if(c == '0' && num_only.isEmpty())	;
		else if(Character.isDigit(c)){
			num_only.append(c);
		}
	}
	ans = num_only.toString();
	
	return ans;
  }
  
  public static void main(String[] args) {
    Problem9 pb9 = new Problem9();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    
    System.out.print(pb9.solution(inString));
      
    return ;
  }
}
```
1. `StringBuilder` 생성
2. 입력받은 문자열에서 첫 문자가 0인지 체크
3. 첫 문자가 '0'이 아닐 경우에 `isDigit()`함수로 숫자만 가져와서 StringBuilder에 담음

## 내코드 #2:
```java
public String solution(String str){
	String ans = "";
	
	StringBuilder num_only = new StringBuilder();
	str = str.replaceAll("[\\D]", ""); //[\\D]는 [^0-9]와 같음
	
	for(char c : str.toCharArray()) {
		if(c == '0' && num_only.isEmpty())	;
		else {
			num_only.append(c);
		}
	}
	ans = num_only.toString();
	
	return ans;
}
```
1. 전에 배운 replaceAll과 정규표현식 활용
2. 문자열에서 숫자만 먼저 뽑기
3. 다시 `StringBuilder`에 담으면서 첫문자가 '0'이 아닐경우에만 담기


## 다른 방법 #1

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(String str){
	int ans = 0;
	
	for(char c : str.toCharArray()) {
		if(c >= 48 && c <= 57) {
			ans = ans * 10 + (c - 48);
		}
	}
	
	return ans;
}
```
- 아스키코드 번호로 숫자만 구분해서 출력
	- '0': 아스키코드 번호 48
	- '9': 아스키코드 번호 57

## 다른 방법 #2

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(String str){
	String ans = "";
	
	for(char c : str.toCharArray()) {
		if(Character.isDigit(c)) {
			ans += c;
		}
	}
	return Integer.parseInt(ans);
}
```
- 내 코드와 동일한 접근
- `Integer.parseInt()` 이용
- 굳이 첫 문자가 '0'인지 찾을 필요 없이 `parseInt()` 메서드를 이용해서 `String`형을 `Int`형으로 변환해 줄 수 있다. 

## 개선된 점:
- 아스키코드로 구현 가능
- `Integer.parseInt` 메서드로 좀 더 쉽게 풀 수 있다.

끝!
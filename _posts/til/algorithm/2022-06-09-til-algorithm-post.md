---
layout: post
title: String \#7. 회문 문자열
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#7. 회문 문자열

* toc
{:toc .large-only}

## 문제: 

> 대소문자를 구분하지 않는 회문 문자열 결과 출력

## 내 코드:

```java
public class Problem7 {
  public String solution(String str){
	String ans = "";
	
	String str_upper = str.toUpperCase();
	StringBuilder sb_reverse = new StringBuilder(str_upper).reverse();
	
	if(str_upper.contentEquals(sb_reverse)) {
		ans = "YES";
	}
	else {
		ans = "NO";
	}
	return ans;
  }
  
  public static void main(String[] args) {
    Problem7 pb7 = new Problem7();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    
    System.out.print(pb6.solution(inString));
      
    return ;
  }
}
```
1. 입력받은 문자열 대문자 변환
2. `StringBuilder`를 이용해 문자열 뒤집기
3. 내용이 같으면 "YES" 아니면 "NO" 반환

## 다른 방법 1

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
	String ans = "YES";
	str = str.toUpperCase();
	int len = str.length();
	for(int i = 0; i < len/2; i++) {
		if(str.charAt(i) != str.charAt(len-i-1)) {
			return "NO";
		}
	}
	
	return ans;
}
```
- for문을 통해 앞 문자와 뒷 문자를 직접 비교하면 시간을 절반으로 줄일 수 있다

## 다른 방법 2

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
	String ans = "NO";
	
	String str_reverse = new StringBuilder(str).reverse().toString();
	
	if(str.equalsIgnoreCase(str_reverse)) {
		ans = "YES";
	}
	return ans;
}
```
- 내가 구현한 방법과 동일
- `str.equalsIgnoreCase()` 함수를 이용하면 대소문자를 무시하고 비교 가능


## 개선된 점:
- 시간이 중요한 문제는 방법1처럼 시간을 절반으로 줄이는 방안을 생각해야 할 것 같다.
- `str.equalsIgnoreCase()` 메서드 기억할 것

끝!
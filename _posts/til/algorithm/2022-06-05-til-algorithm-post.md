---
layout: post
title: String \#4. 단어 뒤집기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#4. 단어 뒤집기

* toc
{:toc .large-only}

## 문제: 

> N개의 단어 뒤집어서 출력

## 내 코드:

```java
package string;

import java.util.Scanner;

public class Problem4 {
  public String solution(String str){
	String ans = null;
	char[] tmp = new char[str.length()];
	char[] charArr = str.toCharArray();
	
	for(int i = 0; i < charArr.length; i++) {
		tmp[i] = charArr[charArr.length - i - 1]; //뒤집기
	}
	ans = String.copyValueOf(tmp);  //char 배열 String 변환
    
	return ans;
  }
  
  public static void main(String[] args) {
    Problem4 pb4 = new Problem4();
    Scanner in = new Scanner(System.in);
    int cnt = in.nextInt();
    String[] inString = new String[cnt];
    
    for(int i = 0; i < cnt; i++) {  //N개 단어 입력받기
    	inString[i] = in.next();
    }
    for(int i = 0; i < cnt; i++) {
    	System.out.println(pb4.solution(inString[i]));
    }
      
    return ;
  }
}
```
1. 입력받은 `String`을 `char[]`로 변환
2. `char[]`을 뒤집기
3. 뒤집은 `char[]`을 String으로 변환하여 출력

## 다른 방법 1

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
	String ans = new StringBuilder(str).reverse().toString();
    
	return ans;
}
```
`StringBuilder`를 사용하면 간단하게 구현할 수 있다.  
1. `StingBuilder` 선언
2. `reverse()` 메서드를 이용해 뒤집기
3. `toString()` 메서드를 이용해 String형으로 변환

## StringBuilder를 사용하는 이유

- `String` 객체는 변경 불가능
- `String`을 연산하게 되면 메모리 할당과 해제가 발생
- 성능적으로 좋지 않음

## StringBuilder 메서드

| 메서드 | 설명 |
| --- | --- |
| `StringBuilder append()` | 기본 자료형 데이터를 문자열에 추가 |
| `StringBuilder delete(int start, int end)` | start ~ end 이전까지 데이터 삭제 |
| `StringBuilder insert(int offset, String str)` | offset 위치에 str에 전달된 문자열 추가 |
| `StringBuilder reverse()` | 저장된 문자열 내용 뒤집기 |
| `StringBuilder substring(int start, int end)` | start ~ end 이전까지의 내용만 담은 String 인스턴스 생성 및 반환 |
| `String toString()` | 저장된 문자열 내용을 담아 String 인스턴스 생성 및 반환 |

## 다른 방법 2

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
	String ans = "";
	char[] charArr = str.toCharArray();
	int lt = 0, rt = str.length() - 1;
    
	while(lt < rt) {
		char tmp = charArr[lt];
		charArr[lt] = charArr[rt];
		charArr[rt] = tmp;
		lt++;
		rt--;
	}
	ans = String.copyValueOf(charArr);
	
	return ans;
}
```
하나씩 거꾸로 하는 방법이 아니라 양 끝에 있는 두 데이터를 바꿔가며 구현하는 것도 가능 


## 개선된 점:
- StringBuilder를 사용하면 굉장히 쉽게 구현 가능하다
- 양 끝 데이터를 바꾸며 구현하는 방법도 있다

끝!
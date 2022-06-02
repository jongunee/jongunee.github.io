---
layout: post
title: String \#3. 문장 속 단어
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

## String \#3. 문장 속 단어

* toc
{:toc .large-only}

## 문제: 

> 문장 속 가장 긴 단어 출력하기

## 내 코드:

```java
public class Problem3 {
  public String solution(String str){
    String ans = "";
    String[] parsed = str.split(" ");
    int max_length = 0;
    int max_idx = 0;
    
    for(int i = 0; i < parsed.length; i++) {
    	if(max_length < parsed[i].length()) {
    		max_length = parsed[i].length();
    		max_idx = i;
    	}
    }
    
    ans = parsed[max_idx];
	  return ans;
  }
  
  public static void main(String[] args) {
    Problem3 pb3 = new Problem3();
    Scanner in = new Scanner(System.in);
    String inString = in.nextLine();
    System.out.print(pb3.solution(inString));
      
    return ;
  }
}
```
`split()` 메서드를 이용해서 파싱을 해주고 가장 길이가 긴 단어가 담긴 인덱스를 찾아 해당 인덱스의 단어를 반환해주었다.

## 다른 방법 1 - 향상된 for문

```java
public String solution(String str){
  String ans = "";
  String[] parsed = str.split(" ");
  int max_length = Integer.MIN_VALUE;	//Int 형의 최소값으로 초기화
  
  for(String s : parsed) {  //향상된 for문
    int len = s.length();
    if(len > max_length) {
      max_length = len;
      ans = s;
    }
  }
  return ans;
}
```
- Integer.MIN_VALUE 최소값으로 초기화하면 좋을듯
- 향상된 for문으로도 구현 가능

## 다른 방법 2 - indexOf(), substring() 메서드

```java
public String solution(String str){
  String ans = "";
  String[] parsed = str.split(" ");
  int max_length= Integer.MIN_VALUE, pos;	//Int 형의 최소값으로 초기화
  
  while((pos = str.indexOf(' ')) != -1) {	//공백 발견시
    String tmp = str.substring(0, pos); // 단어 확인
    int len = tmp.length();
    if(len > max_length) {  //길이 확인
      max_length = len;
      ans = tmp;
    }
    str = str.substring(pos + 1); //공백 이후부터 문자열 다시 구성
  }
  if(str.length() > max_length) ans = str;  //마지막 문자열은 확인이 안되기 떄문에 체크 
  
  return ans;
}
```

## 개선된 점:
- `split()` 메서드 뿐만 아니라 `indxOf()` 메서드와 `substring()` 메서드로 구현 가능

끝!
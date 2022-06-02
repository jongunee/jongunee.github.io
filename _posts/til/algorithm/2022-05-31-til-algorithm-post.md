---
layout: post
title: String \#2. 대소문자 변환
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#2. 대소문자 변환

* toc
{:toc .large-only}

## 문제: 

>문자열 하나를 입력 받고, 대문자는 소문자로, 소문자는 대문자로 변환

## 내 코드:

```java
public class Problem2 {
  public String solution(String str){
    String ans = "";
    
    for(char c : str.toCharArray()){  //향상된 for문
    	if(Character.isUpperCase(c)) {  //대문자리면
    		ans += Character.toLowerCase(c);  //소문자로 변환
    	}
    	else {  //소문자라면
    		ans += Character.toUpperCase(c);  //대문자로 변환
    	}
    }
	    
	return ans;
  }
  
  public static void main(String[] args) {
    Problem2 pb2 = new Problem2();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    System.out.print(pb2.solution(inString));
      
    return ;
  }
}
```
향상된 for문을 사용했고 `Character` 라이브러리의 메서드를 이용해 대소문자를 확인/변환해주었다.

## 다른 방법

```java
for(char c : str.toCharArray()){
  if(c >= 65 && c <= 90) {  //대문자라면
    ans += (char)(c + 32);  //소문자로 변환
  }
  else {  //소문자라면
    ans += (char)(c - 32);  //대문자로 변환
  }
}
```
아스키 코드에서 알파벳은 10진수로 다음과 같기 때문에 

- 'A' ~ 'Z': 65 ~ 90
- 'a' ~ 'z': 97 ~ 122

대문자 범위에 있을 경우 소문자로 변환한다. 대문자와 소문자는 10진수로 <span style='background-color: #f5f0ff'>32</span> 차이가 나기 때문에 32를 더해준다.

## 개선된 점:
- 문제 없이 잘 풀었다
- 아스키 코드를 이용한 구현도 가능하다


끝!
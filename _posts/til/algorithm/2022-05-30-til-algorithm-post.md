---
layout: post
title: String \#1. 문자 찾기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#1. 문자 찾기

* toc
{:toc .large-only}

## 문제: 

>문자열 하나를 입력 받고, 문자 하나를 입력 받아 문자가 문자열에 몇 개 존재하는지 반환. 단, 대소문자를 구분하지 않음

## 내 코드:

```java
public class Problem1 {
  public static void main(String[] args) {

    Scanner in = new Scanner(System.in);
    String inString = in.next().toUpperCase();  //문자열 대문자로 변환
    String inChar = in.next().toUpperCase();  //문자 대문자로 변환
    int cnt = 0;

    for(int i = 0; i < inString.length(); i++) {
      if(inString.charAt(i) == inChar.charAt(0)) {
        cnt++;
      }
    }
    System.out.println(cnt);
    
    return ;
  }
}
```
대소문자를 구분하지 않기 때문에, 문자열과 문자를 받은 뒤에 대문자로 변환시켜 비교


## 개선 코드:

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public class Problem1 {
  public int solution(String str, char c){
    int ans = 0;
    str = str.toUpperCase();
    c = Character.toUpperCase(c);
    
    for(char x : str.toCharArray()){  //향상된 for문
      if(x == c) ans++;
    }
    
    return ans;
  }
  public static void main(String[] args) {
    Problem1 pb1 = new Problem1();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    char inChar = in.next().charAt(0);
    System.out.print(pb1.solution(inString, inChar));
      
    return ;
  }
}
```

## 개선된 점:

- 프로그래머스 동작처럼 메서드 형태로 구현
- 처음에 코드 작성시 두 번째 문자를 입력 받을 때 `char`형을 사용하려 했지만 `char`형도 라이브러리로 대문자 변환 메서드가 존재하는지 몰랐다.  
➡️ `char` 변수에 담아서 작성
- 향상된 `for`문 사용

끝!
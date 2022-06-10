---
layout: post
title: String \#8. 유효한 팰린드롬
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#8. 유효한 팰린드롬

* toc
{:toc .large-only}

## 문제: 

> 팰린드롬인지 판단하는 프로그램 작성  
> ※ 알파벳 이외의 문자들은  무시 

## 내 코드:

```java
public class Problem8 {
  public String solution(String str){
	String ans = "NO";
	
	StringBuilder sb = new StringBuilder();
	String alpha_only = "";
	
	for(char c : str.toCharArray()) {
		if(Character.isAlphabetic(c)){
			sb.append(c);
		}
	}
	alpha_only = sb.toString();
	if(alpha_only.equalsIgnoreCase(sb.reverse().toString())) {
		ans = "YES";
	}
	
	return ans;
  }
  
  public static void main(String[] args) {
    Problem8 pb8 = new Problem8();
    Scanner in = new Scanner(System.in);
    String inString = in.nextLine();
    
    System.out.print(pb8.solution(inString));
      
    return ;
  }
}
```
1. `StringBuilder`에 입력받은 문자열의 알파벳만 담는다
2. `equalsIgnoreCase` 메서드를 이용
3. 앞에서 읽었을 때와 뒤에서 읽었을 때 문자열이 같은지 체크

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
	String ans = "NO";
	
	str = str.toUpperCase().replaceAll("[^A-Z]", "");
	String tmp = new StringBuilder(str).reverse().toString();
	if(str.equals(tmp)) ans = "YES";
	
	return ans;
}
```
- replaceAll()함수를 이용
- \[^A-Z] 의미: 정규표현식, A~Z를 제외한 모든 문자를 의미

## 정규표현식

| 정규표현식 | 설명 |
| --- | --- |
| ^ | 정규식 시작 |
| $ | 정규식 끝 |
| . | 임의의 한 문자 |
| ? | 앞의 문자가 하나 있거나 없거나 |
| * | 앞의 문자가 하나도 없거나 무수히 많거나 |
| + | 잎의 문자가 하나 있거나 무수히 많거나 |
| {} | 문자 나오는 횟수<br> - {n}: 앞의 문자가 n번 나옴<br> - {n,}: 앞의 문자가 적어도 n번 나옴<br> - {n, m}: 앞의 문자가 n번 이상, m번 미만 나옴 |
| [abc] | a, b, 또는 c |
| [^abc] | a, b, c 제외 |
| [a-zA-Z]] | a~z 또는 A~Z 사이의 문자를 포함하고 있는지 확인 |
| [a-d[m-p]] | a~d 또는 m~p 사이의 문자를 포함하고 있는지 확인. 위와 동일 |
| \d | 숫자 (=[0-9])) |
| \D | 숫자를 제외한 모든 숫자 (=[^0-9]) |
| \s | 공백문자 (\t \n \x0B \f \r) |
| \S | 공백문자를 제외한 문자 (=[^\s]) |
| \w | 알파벳 또는 숫자 (=[a-zA-Z0-9]) |
| \W | 알파벳 또는 숫자를 제외한 문자 (=[\w]) |



## 개선된 점:
- 정규표현식에 대해 이해
- `replaceAll()` 메서드와 정규표현식을 이용해서 풀 수도 있다. 

끝!
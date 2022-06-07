---
layout: post
title: String \#5. 특정 문자 뒤집기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#5. 특정 문자 뒤집기

* toc
{:toc .large-only}

## 문제: 

> 문장 속 영어 알파벳만 뒤집기

## 내 코드:

```java
public class Problem5 {
  public String solution(String str){
	String ans = "";
	char[] tmpArr = str.toCharArray();
	int lt = 0, rt = str.length()-1;
	
	while(lt < rt) {
		if(!(Character.isAlphabetic(tmpArr[lt]))) {
			lt++; 
			continue;
		}
		if(!(Character.isAlphabetic(tmpArr[rt]))) {
			rt--;
			continue;
		}
		
		char tmp = tmpArr[lt];
		tmpArr[lt] = tmpArr[rt];
		tmpArr[rt] = tmp;
		
		lt++;
		rt--;
	}
	ans = String.copyValueOf(tmpArr);
	
	return ans;
  }
  
  public static void main(String[] args) {
    Problem5 pb5 = new Problem5();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    System.out.print(pb5.solution(inString));
      
    return ;
  }
}
```
- 인덱스를 이용해 알파벳일 경우에만 양끝 문자를 Swap 해주었다.
- 알파벳이 아닐 경우에는 인덱스를 하나씩 증가/감소 시켜주었다.

## 다른 방법

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

while(lt < rt) {
	if(!(Character.isAlphabetic(tmpArr[lt]))) {
		lt++; 
	}
	else if(!(Character.isAlphabetic(tmpArr[rt]))) {
		rt--;
	}
	
	else{
		char tmp = tmpArr[lt];
		tmpArr[lt] = tmpArr[rt];
		tmpArr[rt] = tmp;

		lt++;
		rt--;
	}
}
```
- 동일한 방식이지만 continue를 사용하지 않고 if - else if - else로 구현 가능



## 개선된 점:

- 문제 없이 잘 풀었다
- continue보다는 if - else로 구현하는 것이 좀 더 가독성 있어 보인다


끝!
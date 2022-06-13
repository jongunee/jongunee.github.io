---
layout: post
title: String \#11. 문자열 압축
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#11. 문자열 압축

* toc
{:toc .large-only}

## 문제: 

> 문자가 반복되는 경우 문자 바로 오른쪽에 반복 횟수 표기해서 출력  
> ※ 반복횟수 1인 경우 생략

## 내 코드:

```java
public class Problem11 {
  public String solution(String str){
    str = " " + str + " ";
    StringBuilder ans = new StringBuilder();
    int cnt = 0;
    for(int i = 1; i < str.length(); i++) {
    	if(str.charAt(i - 1) != str.charAt(i)) {
    		if(cnt > 1) {
    			ans.append(cnt);
    			cnt = 0;
    		}
    		cnt = 0;
    		ans.append(str.charAt(i));
    		cnt++;
    	}
    	else {
    		if(cnt == 0){
    			ans.append(str.charAt(i));
    		}
    		cnt++;
    	}
    }
    return ans.toString();
  }
  public static void main(String[] args) {
    Problem11 pb11 = new Problem11();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    System.out.print(pb11.solution(inString));
      
    return ;
  }
}
```
1. 예외 처리를 위해 `str` 양 끝에 공백 첨부
2. `for`문으로 탐색하면서 전의 문자가 현재 문자와 동일하면 문자열에 문자를 담으면서 카운트 1을 더함
3. 다르면서 카운트가 1보다 크면 카운트값을 문자열에 담음

## 다른 방법:

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
    String ans = "";
    str += " ";
    
    int cnt = 1;
    for(int i = 0; i < str.length() - 1; i++) {
    	if(str.charAt(i)== str.charAt(i + 1)) cnt++;
 
    	else {
    		ans += str.charAt(i);
    		if(cnt > 1) {
    			ans += String.valueOf(cnt);
    			cnt = 1;
    		}
    	}
    }
    return ans;
}
```
- 내 풀이와 같은 접근
- cnt 시작을 1
- i 번째와 i+1 번쨰 비교

## 개선된 점:
- cnt를 1로 시작하는 것이 코드 간소화가 가능할 것 같다.
- 굳이 i-1 번쨰와 비교할 필요는 없었던 것 같다.

끝!
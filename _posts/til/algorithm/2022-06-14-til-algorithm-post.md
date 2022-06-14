---
layout: post
title: String \#12. 암호
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#12. 암호

* toc
{:toc .large-only}

## 문제: 

> 다음 절차에 따라 암호 해석
> 1. 문자 \#은 1로, 문자 \*은 0으로 변환
> 2. 2진수 10진수화
> 3. 아스키 번호에 따라 문자 변환  
> ※ 문자는 7글자씩 끊어서 해석

## 내 코드:

```java
public String solution(int n, String str){
    String[] tmp = new String[n];
    int[] intArr = new int[n];
    StringBuilder ans = new StringBuilder();
    for(int i = 0; i < n; i++) {
    	tmp[i] = "";
    }
    int cnt = -1;
    for(int i = 0; i < str.length(); i++) {
    	if(i % 7 == 0) cnt++;
    	
    	if(str.charAt(i) == '#') {
    		tmp[cnt] += 1;
    	}
    	else if(str.charAt(i) == '*') {
    		tmp[cnt] += 0;
    	}
    }
    for(int i = 0; i < n; i++) {
    	intArr[i] = Integer.valueOf(tmp[i], 2); 
    	ans.append((char)intArr[i]);
    }
    return ans.toString();
  }
```
1. 7자씩 끊어서 카운트
2. 문자 \#은 1로, 문자 \*은 0으로 변환
3. 2진수 변환
4. `char`형으로 변환해서 추가

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(int n, String str){
    String ans = "";

    for(int i = 0; i < n; i++) {
    	String temp = str.substring(0, 7).replace('#', '1').replace('*', '0');
    	int num = Integer.parseInt(temp, 2);
    	ans += (char)num;
    	str = str.substring(7);
    }
    
    return ans;
  }
```
1. `substring()` 메서드 사용
2. `replace()` 메서드 사용
3. `parseInt()` 메서드 사용

## 개선된 점:
- 접근은 비슷하지만 사용한 메서드가 다름
- 코드를 훨씬 간소화가 가능하다
- 다른 메서드도 기억할 것...

끝!
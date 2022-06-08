---
layout: post
title: String \#6. 중복문자제거
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#6. 중복문자제거

* toc
{:toc .large-only}

## 문제: 

> 중복된 문자를 제거 - 문자열의 같은 문자가 두 번 이상 등장하지 않도록 하며 순서는 유지.  
> `ksekkset` 입력시 `kset`를 출력하도록 한다. 

## 내 코드:

```java
public class Problem6 {
  public String solution(String str){
	char charArr[] = str.toCharArray();
	StringBuilder ans = new StringBuilder();
	HashMap<Character, Integer> check = new HashMap<>();
	
	for(int i = 0; i < charArr.length; i++) {
		if(check.containsKey(charArr[i])) continue;	//value가 이미 존재하면 skip
		else {
			check.put(charArr[i], 1);	//value가 없으면 체크 의미로 1 삽입
			ans.append(charArr[i]);	//ans StringBuilder에 문자 삽입
		}
	}
	
	return ans.toString();
  }
  
  public static void main(String[] args) {
    Problem6 pb6 = new Problem6();
    Scanner in = new Scanner(System.in);
    String inString = in.next();
    
    System.out.print(pb6.solution(inString));
      
    return ;
  }
}
```
1. 입력받은 `String`을 `char[]`로 변환
2. `HashMap` 구현해서 문자 확인하고 체크
	- Key: 문자열의 문자
	- Value: 이미 들어와있는지 체크 - 1이 담겨있으면 이미 한 번 문자가 등장했다는 것
3. HashMap에서 Key에 따라 value가 없으면 ans에 문자를 담고 value가 존재하면 skip  
	➡️문자를 한 번만 담게됨 

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str){
	String ans = "";
	
	for(int i = 0; i < str.length(); i++) {
		if(str.indexOf(str.charAt(i)) == i) 
			ans += str.charAt(i);
	}
	
	return ans;
}
```

- `str.indexof(str.charAt(i))`: str의 i번째 문자가 처음 등장하는 index를 반환해준다.
- `str.indexof(str.charAt(i))`와 i가 같다는 것은 처음 등장했다는 의미 - 중복되지 않는다는 의미
- `str.indexOf(str.charAt(i)) == i` 일 때만 문자열에 문자를 담아주면 된다.

## 개선된 점:
- HashMap을 이용해도 가능하지만 `indexof()`를 이용하면 훨씬 간단하게 풀 수 있는 문제였다.
- 기억하자... `indexOf()`...

끝!
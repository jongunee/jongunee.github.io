---
layout: post
title: String \#10. 가장 짧은 문자거리
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# String \#10. 가장 짧은 문자거리

* toc
{:toc .large-only}

## 문제: 

> 문자열 s의 각 문자와 t 사이의 최소거리 출력

## 내 코드:

```java
public class Problem10 {
  public void solution(String str, char c){
	char[] charArr = str.toCharArray();
	ArrayList<Integer> index = new ArrayList<>();
	ArrayList<Integer> ans = new ArrayList<>();
	
	int lt = 0, rt = 0;
	for(int i = 0; i < charArr.length; i++) {
		ans.add(i);
		if(charArr[i] == c) {
			index.add(i);
		}
	}
	if(!index.isEmpty()) {
		rt = index.get(0);
		while(rt >= lt) {
			ans.set(lt, rt - lt);
			
			lt++;
		}
		for(int i = 0; i < index.size(); i++) {
			if(i == index.size() - 1) {
				lt = index.get(i);
				rt = str.length() - 1;
				while(rt >= lt) {
					ans.set(rt, rt - lt);
					
					rt--;
				}
			}
			else {
				lt = index.get(i);
				rt = index.get(i + 1);
				while(rt >= lt) {
					ans.set(lt, lt - index.get(i));
					ans.set(rt, index.get(i + 1) - rt);
					
					lt++; rt--;
				}
			}
		}
		
	}
	for(int i : ans) {
		System.out.print(i + " ");
	}
  }
  
  public static void main(String[] args) {
    Problem10 pb10 = new Problem10();
    Scanner in = new Scanner(System.in);
    String s = in.next();
    char t = in.next().charAt(0);
    
    pb10.solution(s, t);
      
    return ;
}
```
1. 문자열 s에 문자 t가 존재하는지 확인
2. `arrayList`에 문자 t의 인덱스를 담음
3. 인덱스에 따라 거리 측정
	- 0 ~ 첫 번째 인덱스 
	- 첫 번쨰 인덱스 ~ 마지막 인덱스
	- 마지막 인덱스 ~ `str.length()`

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public void solution(String str, char c){
	int[] ans = new int[str.length()];
	int p = 1000;
	for(int i = 0; i <str.length(); i++) {
		if(str.charAt(i) == c) {
			p = 0;
			ans[i] = p;
		}
		else {
			p++;
			ans[i] = p;
		}
	}
	p = 1000;
	for(int i = str.length() - 1; i >= 0; i--) {
		if(str.charAt(i) == c) {
			p = 0;
		}
		else {
			p++;
			ans[i] = Math.min(ans[i], p);
		}
	}
	
	for(int i : ans) {
		System.out.print(i + " ");
	}
}
```
- p라는 변수에 큰 값 1000 대입(문자열 길이 < 100)
- 왼쪽에서부터 for문
	- 문자 t를 만나면 p를 0으로 만들고 p 대입
	- 아니면 p를 1 증가 후 p 대입
- 오른쪽에서부터 for문 한 번 더
	- 문자 t를 만나면 p를 0으로 만듬
	- 아니면 p를 1 증가 후 기존의 값과 비교해서 작으면 대입

## 개선된 점:
- 구역을 3군데로 나누어서 접근을 해서 딱봐도 너무 복잡하게 푼 느낌
- 풀이를 보니 두 번 탐색해서 코드를 좀 더 간소화 시킬 수 있다 

끝!
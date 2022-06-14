---
layout: post
title: Array \#1. 큰 수 출력하기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#1. 큰 수 출력하기

* toc
{:toc .large-only}

## 문제: 

> 자연수 N개가 주어지며 자신의 바로 앞 수보다 큰 수만 출력  
> ※ 첫 번쨰 수는 무조건 출력

## 내 코드:

```java
public String solution(int n, int num[]){
  String ans = "";
  ans += num[0];
  for(int i = 1; i < n; i++) {
    if(num[i] > num[i - 1]) ans = ans + " " + num[i];
  }
  return ans;
}
```
1. 첫 번쨰 문자 추가
2. 앞 수보다 클 경우에만 추가 후 출력

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public ArrayList<Integer> solution(int n, int num[]){
  ArrayList<Integer> ans = new ArrayList<>();
  ans.add(num[0]);
  for(int i = 1; i < n; i++) {
    if(num[i] > num[i - 1]) ans.add(num[i]);
  }
  return ans;
}
```
- 내 코드와 같은 방법
- 대신 `ArrayList` 사용


## 개선된 점:
- 간단한 문제
- `ArrayList`로도 구현 가능

끝!
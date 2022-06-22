---
layout: post
title: 효율성 \#1. 두 배열 합치기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# 효율성 \#1. 두 배열 합치기

* toc
{:toc .large-only}

## 문제: 

> 오름차순으로 정렬이 된 두 배열이 주어졌을 때 두 배열을 오름차순으로 합쳐 출력


## 내 코드:

```java
public ArrayList<Integer> solution(int[] arr1, int[] arr2){
  ArrayList<Integer> ans = new ArrayList<Integer>();
  
  for(int i : arr1) {
    ans.add(i);			
  }
  for(int i : arr2) {
    ans.add(i);			
  }
  ans.sort(Comparator.naturalOrder());

  return ans;
}
```

1. `ArryList` 생성
2. `ArrayList`에 배열 2개 합치기
3. 오름차순 정렬

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public ArrayList<Integer> solution(int[] arr1, int[] arr2){
  ArrayList<Integer> ans = new ArrayList<Integer>();
  int idx1 = 0, idx2 = 0, cnt = 0;
  while(idx1 < arr1.length && idx2 < arr2.length) {
    if(arr1[idx1] > arr2[idx2]) ans.add(arr2[idx2++]);
    else ans.add(arr1[idx1++]);
  }
  while(idx1 < arr1.length) ans.add(arr1[idx1++]);
  while(idx2 < arr2.length) ans.add(arr2[idx2++]);

  return ans;
}
```
- 2개 포인터를 잡아서 값을 비교
- 값을 담을 때 작은 것만 담고 포인터 증가

## 개선된 점:
- 합쳐서 정렬하는 것은 뻔한 답이라고 한다
- Two pointers algorithm 사용해보자!


끝!
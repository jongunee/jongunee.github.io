---
layout: post
title: Map/Set \#3. 매출액의 종류
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Map/Set \#3. 매출액의 종류

* toc
{:toc .large-only}

## 문제: 

> N일 동안의 매출 기록이 주어졌을 때 연속된 K일 동안의 매출액 종류 개수 반환
> ※ 매출 기록이 같으면 매출액 종류가 같다


## 내 코드:

```java
public ArrayList<Integer> solution(int n, int k, int[] sales){
  ArrayList<Integer> ans = new ArrayList<>();
  int first = 0;
  Map<Integer, Integer> map = new HashMap<Integer, Integer>();
  
  for(int i = 0; i < k-1; i++) {
    map.put(sales[i], map.getOrDefault(sales[i], 0) + 1);
  }
  for(int i = k-1; i < n; i++) {
    map.put(sales[i], map.getOrDefault(sales[i], 0) + 1);
    ans.add(map.size());
    map.put(sales[first], map.get(sales[first]) - 1);
    if(map.get(sales[first]) == 0) map.remove(sales[first]);
    first++;
  }
  
  return ans;
}
```

1. `Map` 생성
2. `k-1` 개수 만큼 먼저 세팅
3. 다음날 매출액 추가해서 `Map`에 카운트 후 종류 계산
    - 종류 계산은 `Map`의 size로 구함
4. 구간의 첫날 매출엑 및 종류 `Map`에서 제거



## 정답:

- 내 답과 완전 동일


## 개선된 점:
- 잘 풀었다!

끝!
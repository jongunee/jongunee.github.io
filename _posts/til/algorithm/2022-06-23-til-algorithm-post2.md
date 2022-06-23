---
layout: post
title: 효율성 \#4. 연속 부분수열
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# 효율성 \#4. 연속 부분수열

* toc
{:toc .large-only}

## 문제: 

> N개의 수로 이루어진 수열이 주어졌을 때 연속부분수열의 합이 M이 되는 경우 반환  
> 예) 1 2 1 3 1 1 1 2의 연속부분수열은 {2, 1, 3}, {1, 3, 1, 1}, {3, 1, 1, 1}


## 내 코드:

```java
public int solution(int n, int m, int[] arr){
  int idx = 0, sum = 0, ans = 0;
  ArrayList<Integer> check = new ArrayList<>();
  
  while(idx < n) {
    if(sum < m) {
      check.add(arr[idx]);
      sum += arr[idx];
      idx++;
    }
    else if(sum == m) {
      ans++;
      sum -= check.get(0);
      check.remove(0);
    }
    else {
      sum -= check.get(0);
      check.remove(0);
    }
  }
  if(sum == m) ans++;
  
  return ans;
}
```

1. `ArrayList` 생성
2. 합 < M: 원소 담기
3. 합 > M: 가장 앞 원소 뺴기
4. 합 == M: 카운트, 가장 앞 원소 빼기
5. 인덱스가 N이 되면 오류가 나기때문에 `while`문이 끝난 뒤 마지막으로 합과 M인지 확인하고 답 반환

## 다른 풀이:

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(int n, int m, int[] arr){
  int lt = 0, sum = 0, ans = 0;
  
  for(int rt = 0; rt < n; rt++) {
    sum += arr[rt];
    if(sum == m) ans++;
    while(sum > m) {
      sum -= arr[lt++];
      if(sum == m) ans++;
    }
  }
  return ans;
}
```
- 내 코드와 방식은 비슷
- `lt`, `rt`를 이용해 구현

## 개선된 점:
- `lt`, `rt` 투포인터 알고리즘으로 구현할 수도 있다
- 잘 풀었다!

끝!
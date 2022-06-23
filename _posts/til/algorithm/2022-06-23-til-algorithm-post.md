---
layout: post
title: 효율성 \#3. 최대 매출
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# 효율성 \#3. 최대 매출

* toc
{:toc .large-only}

## 문제: 

> N일 동안의 매출 기록이 주어졌을 때 연속된 k일 동안의 최대 매출 구하기


## 내 코드:

```java
public int solution(int n, int k, int[] arr){
  int sum = 0, max = 0;
  for(int i = 0; i < k; i++) {
    sum += arr[i];
    max = sum;
  }
  for(int i = k; i < n; i++) {
    sum -= arr[i-k];
    sum += arr[i];
    max = Math.max(max, sum);
  }
  
  return max;
}
```

1. `for`문을 돌면서 k일 동안의 매출을 더한다
2. 다음날로 넘어갈 때 처음날의 매출은 뺴고 다음날의 매출을 더한다
    - ex) `sum`에 2, 3, 4일 매출이 담겨있다면 2일 매출을 뺴고 5일 매출을 더한다
3. 최대값 반환

## 다른 방법 

```java
for(int i = 0; i < k; i++) {
  sum += arr[i];
}
max = sum;
```
- `max` 값은 `for`문을 빠져나와 넣어주는게 연산 수를 줄일 수 있음
- 말고는 없음

## 개선된 점:

- 내 풀이와 동일
- 잘 풀었다!


끝!
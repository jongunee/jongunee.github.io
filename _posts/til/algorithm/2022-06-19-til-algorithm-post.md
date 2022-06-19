---
layout: post
title: Array \#9. 격자판 최대합
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#8. 등수구하기

* toc
{:toc .large-only}

## 문제: 

> N * N 격자판이 주어졌을 때 각 행의 합, 각 열의 합, 각 대각선의 합 중 가장 큰 합 출력


## 내 코드:

```java
public int solution(int n, int[][] arr){
  int sum = 0;
  int max = 0;
  
  //행 총합
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      sum += arr[i][j];
    }
    max = Math.max(max, sum);
    sum = 0;
  }
  //열 총합
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      sum += arr[j][i];
    }
    max = Math.max(max, sum);
    sum = 0;
  }
  //대각선 총합
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      if(i == j) sum += arr[i][j];
    }
  }
  max = Math.max(max, sum);
  sum = 0;
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      if(i + j == n - 1) sum += arr[i][j];
    }
  }
  max = Math.max(max, sum);
  
    return max;
}
```

1. 행 총합 구하기
2. 열 총합 구하기
3. 대각선 총합 구하기
4. `max` 메서드를 이요해 가장 큰 값만 변수에 담기
5. 가장 큰 값 반환 


## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(int n, int[][] arr){
  int ans = Integer.MIN_VALUE;
  int sum1, sum2;
  
  for(int i = 0; i < n; i++) {
      sum1 = sum2 = 0;
    for(int j = 0; j < n; j++) {
      sum1 += arr[i][j];
      sum2 += arr[j][i];
    }
    ans = Math.max(ans, sum1);
    ans = Math.max(ans, sum2);
  }
  sum1 = sum2 = 0;
  for(int i = 0; i < n; i++) {
    sum1 += arr[i][i];
    sum2 += arr[i][n - i - 1];
  }
  ans = Math.max(ans, sum1);
  ans = Math.max(ans, sum2);
  
    return ans;
}
```
- 방식은 동일
- 행 총합 구할 때 열 총합 같이 구하기
- 대각선 총합 구할 때 2개 같이 구하기

## 개선된 점:
- 시간을 신경ㅆ지 않았다
- 행 총합 구할 때 열 총합도 같이 구할 수 있다
- 대각선 총합 구할 때 굳이 이중 `for`문 돌릴 필요 없음
- 대각선 2개도 하나의 `for`문으로 구할 수 있다.

끝!
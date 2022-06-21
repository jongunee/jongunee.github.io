---
layout: post
title: Array \#11. 임시 반장 정하기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#11. 임시 반장 정하기

* toc
{:toc .large-only}

## 문제: 

> 첫 줄에 학생 수(N)를 입력 받고 둘쨰 줄부터 1학년부터 5학년까지 몇 반에 속했는지 (arr[N][5])입력 받는다. 한 번이라도 같은 반이 되었던 적이 많은 학생이 임시 반장이 될 때 임시반장의 번호 출력  
> ※ 학생의 번호는 1부터 시작, 임시 반장이 될 수 있는 학생이 여러 명이면 가장 작은 번호 출력


## 내 코드:

```java
public int solution(int n, int[][] arr){
  int[][] mat = new int[n][n];
  int cnt = 0, max = 0, ans = 0;
  ArrayList<Integer> arrList = new ArrayList<>();
  
  for(int i = 0; i < n; i++) {
    for(int j = i + 1; j < n; j++) {
      for(int k = 0; k < 5; k++) {
        if(arr[i][k] == arr[j][k]) {
          mat[i][j] = mat[j][i] = 1;
          break;
        }
      }
    }
  }
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      cnt += mat[i][j];
    }
    arrList.add(cnt);
    cnt = 0;
  }
  for(int i = 0; i < n; i++) {
    max = Math.max(max, arrList.get(i));
  }
  ans = arrList.indexOf(max) + 1;

  return ans;
}
```

1. n*n 배열 선언
2. 각 학생끼리 받은 반이 되었던 적이 있으면 1 대입
3. 행(학생 번호) 별로 총합 구하기
4. 가장 큰 값을 가진 학생 번호 반환

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(int n, int[][] arr){
  int ans = 0, max = Integer.MIN_VALUE;
  for(int i = 1; i <= n; i++) {
    int cnt = 0;
    for(int j = 1; j <= n; j++) {
      for(int k = 1; k <= 5; k++) {
        if(arr[i][k] == arr[j][k]) {
          cnt++;
          break;
        }
      }
    }
    if(cnt > max) {
      max = cnt;
      ans = i;
    }
  }

  return ans;
}
```
- 풀이 접근은 비슷
- `for`문을 여러 번 사용해 모든 학생을 비교하는 것이 아니라 같은 반인게 확인되면 break



## 개선된 점:
- 내 풀이는 다 같은 반인지 모두 확인하고 답을 구했는데 확인 도중에 답을 도출할 수 있음
- 풀이를 간소화 시켜보자!

끝!
---
layout: post
title: Array \#12. 멘토링
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#12. 멘토링

* toc
{:toc .large-only}

## 문제: 

> N명의 학생이 M 번 시험을 봤을 떄 등 수가 입력된다. 학생들의 점수를 비교해서 한 학생이 모든 시험에서 다른 한 학생보다 점수가 앞서면 그 둘은 멘토, 멘티로 짝을 짓는다. 이 때 짝을 지을 수 있는 경우 출력 


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

1. 학생별 등수 데이터를 저장할 배열을 생성
2. 행에는 학생 번호, 열에는 시험 번호로 놓고 학생의 등수를 담음
3. 학생끼리 비교해서 모든 등수가 높은 경우 카운트 후 반환

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(int sNum, int tNum, int[][] score){
  int ans = 0;
  for(int i = 1; i <= sNum; i++) {
    for(int j = 1; j <= sNum; j++) {
      int cnt = 0;
      for(int k = 0; k < tNum; k++) {
        int pi = 0, pj = 0;
        for(int s = 0; s < sNum; s++) {
          if(score[k][s] == i) pi = s;
          if(score[k][s] == j) pj = s;
        }
        if(pi < pj) cnt++;
      }
      if(cnt == tNum) ans++;
    }
  }

  return ans;
}
```
- 4중 `for`문 사용
- 굳이 학생 등수 따로 배열에 담지 않고 입력된 배열에서 인덱스 위치로 비교



## 개선된 점:
- 4중 `for`문을 이용해 인덱스로 비교 가능
- 너무 복잡하다.. 

끝!
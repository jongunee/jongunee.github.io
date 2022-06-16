---
layout: post
title: Array \#4. 피보나치 수열
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#4. 피보나치 수열

* toc
{:toc .large-only}

## 문제: 

> N개의 피보나치 수열 항 출력하기
> ※ 3 <= N <= 45, 첫쨰항, 둘째항은 1로 시작


## 내 코드:

```java
public String solution(int n){
  String ans = "1 1 ";
  
  int fibo[] = new int[n];
  fibo[0] = 1;
  fibo[1] = 1;
  
  for(int i = 2; i < n; i++) {
    fibo[i] = fibo[i-1] + fibo[i-2];
    ans = ans + fibo[i] + " ";
  }
  return ans;
}
```

1. 맨 처음 2개 항에 1씩 넣고 시작
2. N-2 번 연산해서 배열에 추가  

## 다른 방법 

- 없음
- 내 풀이와 동일


## 개선된 점:
- 쉬운 문제였다
- 잘 풀었다

끝!
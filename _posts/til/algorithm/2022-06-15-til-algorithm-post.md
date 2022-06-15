---
layout: post
title: Array \#2. 보이는 학생
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#2. 보이는 학생

* toc
{:toc .large-only}

## 문제: 

> N명의 학생들이 일렬로 선생님 앞에 서 있다. 학생들의 키가 앞에서부터 순서대로 주어질 때 선생님이 볼 수 있는 학생의 수 출력  
> ※ 키가 클 떄만 보이고 같을 때는 보이지 않음


## 내 코드:

```java
public int solution(int n, int num[]){
  int ans = 1;
  int max = num[0];
  for(int i = 1; i < n; i++) {
    if(num[i] > max) {
      max = num[i];
      ans++;
    }
  }
  return ans;
}
```
1. 맨 앞에 학생은 항상 보이기 때문에 1부터 카운트
2. 두 번째부터 탐색하면서 가장 큰 키를 `max` 변수에 담음
3. `max` 보다 클 경우에만 1씩 증가  

## 다른 방법 

- 없음
- 내 풀이와 동일


## 개선된 점:
- 쉬운 문제였다
- 잘 풀었다

끝!
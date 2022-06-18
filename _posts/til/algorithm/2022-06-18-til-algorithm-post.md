---
layout: post
title: Array \#7. 점수 계산
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#7. 점수 계산

* toc
{:toc .large-only}

## 문제: 

> OX 문제 점수 계산을 한다. 틀리면 0점 맞추면 1점이지만 연속으로 맞춘 수만큼 점수 획득


## 내 코드:

```java
public int solution(int[] score){
  int ans = 0, cnt = 0;
  
  if(score[0] == 1) {
    ans++;
    cnt++;
  }
  
  for(int i = 1; i < score.length; i++) {
    if(score[i] == 0) cnt = 0;
    else if(score[i] == 1) {
      cnt++;
      ans += cnt;
    }
  }
		
  return ans;
}
```

1. 첫 번쨰 문제 맞췄을 때 총점 1, 카운트 1로 시작
2. `for`문
    - 틀리면 점수 획득 X, 카운트 초기화
    - 맞추면 카운트 1씩 더해 누적하고 카운트한 값을 점수로 획득


## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(int[] score){
  int ans = 0, cnt = 0;
  
  for(int i = 0; i < score.length; i++) {
    if(score[i] == 1) {
      cnt++;
      ans += cnt;
    }
    else cnt = 0;
  }
  
  return ans;
}
```
- 같은 동작 방식
- 하지만 0번 인덱스부터 시작
- `else if` 말고 `else`로 구현



## 개선된 점:
- 처음에 코드 짤 때 이전 인덱스 값과 비교하려고 첫 번째 문제 맞췄을 때 계산을 따로 해줬었는데 그럴 필요가 없었다
- `else if` 보다 `if-else`로 코드를 구현하니까 좀 더 깔끔해 보임

끝!
---
layout: post
title: Array \#5. 소수(에라토스테네스 체)
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#5. 소수(에라토스테네스 체)

* toc
{:toc .large-only}

## 문제: 

> N 이하의 소수 개수 출력


## 내 코드:

```java
public int solution(int n){
  int ans = 0;
  ArrayList<Integer> arr = new ArrayList<>();
  arr.add(2);
  for(int i = 3; i <= n; i += 2) {
    for(int j = 2; j < i; j++) {
      if(i % j == 0) break;
      else if(j == i-1) arr.add(i);
    }
  }
  ans = arr.size();
    
  return ans;
}
```

1. 소수 계산을 위해 N을 2부터 N-1까지 나눠본다
2. 연산 도중 나눠 떨어지는게 있으면 for문을 빠져나옴
3. 끝까지 연산을 하면 소수라고 판단
4. 2로 나누어 떨어지지 않으면 다른 짝수로도 나누어 떨어지지 않기 때문에  바깥 for문에서 i += 2를 해주어 시간을 줄였다
5. 하지만 시간 초과...

## 내 코드2

```java
public int solution(int n){
  int ans = 0;
  int[] sosu = new int[n + 1];
  for(int i = 2; i <= n; i++) {
    if(sosu[i] == 0) {
      ans++;
      for(int j = i; j <= n; j += i) sosu[j]++;
    }
    else continue;
  }
    
  return ans;
}
```
에라토스테네스 체 설명 듣고 코드 작성 - 해설과 코드 동일
1. 배열 선언시 모두 0으로 초기화
2. 2부터 탐색하며 배열에 체크가 되어 있지 않으면 소수라고 판단
3. 어떤 수가 약수가 있으면 소수가 아니기 때문에 소수라고 판단된 숫자의 배수를 모두 체크
4. 2, 3을 반복하며 체크가 되어있으면 소수이기 때문에 스킵


## 개선된 점:
- 에라토스테네스 체에 대해 알고 있었다면 쉬운 문제
- 기억하자.. 에라토스테네스

끝!
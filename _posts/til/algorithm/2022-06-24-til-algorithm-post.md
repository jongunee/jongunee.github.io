---
layout: post
title: 효율성 \#5. 연속된 자연수의 합
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# 효율성 \#5. 연속된 자연수의 합

* toc
{:toc .large-only}

## 문제: 

> 양의 정수 n을 2개 이상의 연속된 자연수 합으로 표현 가능한 방법 가짓수 출력  
> 예) 15: {1, 2, 3, 4, 5}, {4, 5, 6}, {7, 8} ➡️3가지


## 내 코드:

```java
public int solution(int n){
  int num = 1, ans = 0, sum = 0;
  ArrayList<Integer> arr = new ArrayList<>();
  
  while(num < n) {
    if(sum < n) {
      arr.add(num);
      sum += num;
      num++;
    }
    else if(sum == n) {
      ans++;
      if(arr.size() == 2) break;
      
      sum -= arr.get(0);
      arr.remove(0);
    }
    else {
      sum -= arr.get(0);
      arr.remove(0);
    }
  }
  
  return ans;
}
```

1. `ArrayList` 생성 후 `while`문을 돌린다
2. 합 < N: `ArrayList`에 담기, 합 구하기, 숫자 증가
3. 합 == N: 답 카운트, 가장 먼저들어온 값 제거, 합에서 뺴기
4. 합 > N: 가장 먼저들어온 값 제거, 합에서 뺴기
5. 연속된 2개 이상의 자연수 합으로 표현해야하기 때문에 합==N 이면서 `ArrayList`에 자연수가 2개 들어있으면 `break`로 빠져나온다 

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(int n){
  int ans = 0, cnt = 1;
  n--;
  while(n > 0) {
    cnt++;
    n = n - cnt;
    if(n % cnt == 0) ans++;
  }
  
  return ans;
}
```
- 수학적인 방법으로 풀이
- 연속해야하기 떄문에 연속된 숫자 중 가장 작은 1부터 시작해서 깔아놓고 그 뒤에 같은 숫자를 더해주면 된다
  - ex) 합이 15인 연속된 2 숫자를 구하려면 1, 2를 먼저 깐다
  - 15에서 1, 2에 합인 3을 빼면 12가 남음
  - 값을 동등하게 더해주기 위해 12를 2로 나눠주면 6
  - 1, 2에 6씩 동등하게 더해주면 7, 8이 나옴 

## 개선된 점:

- 수학적 계산으로도 풀 수 있다
- 다양하게 생각해 봐야겠다


끝!
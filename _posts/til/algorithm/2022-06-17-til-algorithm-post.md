---
layout: post
title: Array \#6. 뒤집은 소수
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#6. 뒤집은 소수

* toc
{:toc .large-only}

## 문제: 

> N개의 숫자가 입력되었을 때 뒤집은 수가 소수면 출력
> ※ 뒤집었을 때 앞자리가 0이면 무시 ex) 200을 뒤집으면 2

## 내 코드:

```java
public String solution(String n){
    String ans = "";
    StringBuilder sb = new StringBuilder(n);
    int nInverse = Integer.parseInt(sb.reverse().toString());
    int sosu[] = new int[nInverse + 1];

    if(nInverse == 1) return ans;
    else if(nInverse == 2) return "2 ";

    for(int i = 2; i < nInverse; i ++) {
        if(nInverse % i == 0) return ans;
    }
    ans = Integer.toString(nInverse) + " ";

    return ans;
}
```
1. 숫자를 `String` 형으로 받음
2. `StringBuilder`로 뒤집은 뒤 `Int` 형 변환
3. `for`문 돌려서 약수 없으면 소수
3. 소수일 경우에만 출력

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public boolean isPrime(int num) {
  if(num == 1) return false;
  for(int i = 2; i < num; i++) {
    if(num % i == 0) return false;
    
  }
  return true;
}
	
public ArrayList<Integer> solution(int n, int[] arr){
  ArrayList<Integer> ans = new ArrayList<>();
  for(int i = 0; i < n; i++) {
    int tmp = arr[i];
    int res = 0;
    while(tmp > 0) {
      int t = tmp % 10;
      res = res * 10 + t;
      tmp = tmp/10;
    }
    if(isPrime(res)) ans.add(res);
  }
  
  return ans;
}
```

- 소수 판별 방식 동일
- 숫자 뒤집는 방식이 다름


## 개선된 점:
- 수식을 이용해서 숫자를 뒤집을 수도 있다

끝!
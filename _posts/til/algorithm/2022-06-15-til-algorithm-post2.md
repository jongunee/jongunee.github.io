---
layout: post
title: Array \#3. 가위 바위 보
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#3. 가위 바위 보

* toc
{:toc .large-only}

## 문제: 

> A, B가 가위바위보 게임을 할 때 A가 이기면 A 출력, B가 이기면 B 출력, 비기면 D 출력한다. 게임 횟수, A 정보, B 정보가 주어질 때 결과 출력  
> ※ 가위: 1, 바위: 2, 보: 3

## 내 코드:

```java
public char solution(int a, int b){
  char ans;
  char result[][] = {
      {'X', 'X', 'X', 'X'},
      {'X', 'D', 'B', 'A'},
      {'X', 'A', 'D', 'B'},
      {'X', 'B', 'A', 'D'}
  };
  ans = result[a][b];
  
  return ans;
}
```
1. 2명 뿐이기 때문에 결과를 예상할 수 있다.
2. 편의를 위해 4행 4열 배열을 만들고 0에 해당하는 행과 열은 'X'로 표시

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(int n, int a[], int b[]){
  String ans = "";
  for(int i = 0; i < n; i++) {
    if(a[i] == b[i]) ans += "D";
    else if(a[i] == 1 && b[i] == 3) ans += "A";
    else if(a[i] == 2 && b[i] == 1) ans += "A";
    else if(a[i] == 3 && b[i] == 2) ans += "A";
    else ans += "B";
  }
    
  return ans;
}
```
- `if`문 사용해서 문제 해결


## 개선된 점:
- 굳이 `if`문을 사용하지는 않았다
- 여러가지 풀이법이 존재한다

끝!
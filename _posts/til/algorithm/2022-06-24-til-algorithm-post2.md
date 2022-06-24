---
layout: post
title: 효율성 \#6. 최대 길이 연속부분수열
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# 효율성 \#6. 최대 길이 연속부분수열

* toc
{:toc .large-only}

## 문제: 

> 길이가 N이고 0과 1로 구성된 수열이 주어진다. 최대 k번 0을 1로 변경 가능할 때 1로만 구성된 최대 길이의 연속부분수열의 길이 반환


## 내 코드:

```java
public int solution(int n, int k, int[] arr){
  int ans = 0;
  ArrayList<Integer> arrList = new ArrayList<>();
  
  for(int i = 0; i < arr.length; i++) {
    if(arr[i] == 0) arrList.add(i);
  }
  arrList.add(arr.length);
  
  for(int i = 0; i < arrList.size()-k-1; i++) {
    ans = Math.max(ans, arrList.get(i+k+1) - arrList.get(i)-1);
  }
  
  return ans;
}
```

1. 0의 인덱스를 `ArrayList`에 담기
2. 수열이 1로 끝날 수도 있기 때문에 arrList에 `최대 인덱스 + 1` 값을 넣어준다
3. 0을 k번 1로 바꾸고도 남아있는 0의 인덱스를 찾아 연속된 1의 길이의 최대값을 구한다
4. 예) 1 1 0 0 1 1 0 1 1 0 1 1 0 1
    - 0 인덱스는 2, 3, 6, 9, 12, 16(임의로 대입)
    - 최대 길이는 인덱스 6, 9를 1로 변경했을 때
    -  4 ~ 11 ➡️길이 8 (12 - 3 - 1)


## 다른 풀이:

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조


```
1. 길이: `rt` - `lt` + 1
2. `rt`가 0을 만나면 1로 바꿔주고 카운트
3. `rt`가 0을 만났는데 카운트 값이 `k`보다 크면
    - 가장 먼저 1로 변경된 값을 0으로 돌려놓음
    - 카운트 값 - 1 

## 개선된 점:
- 투포인터 알고리즘으로도 풀 수 있다
- 전혀 생각도 못한 방법, 다양하게 익혀보자

끝!
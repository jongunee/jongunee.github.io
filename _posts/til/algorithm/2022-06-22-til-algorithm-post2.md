---
layout: post
title: 효율성 \#2. 공통원소 구하기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# 효율성 \#2. 공통원소 구하기

* toc
{:toc .large-only}

## 문제: 

> 두 숫자 집합이 주어졌을 때 공통 원소를 추출해서 오름차순 출력  
> 각 집합의 원소는 중복되지 않음
> ※ 집합의 크기는 최대 30000, 시간 제한 1000ms


## 내 코드 방법 1:

```java
public ArrayList<Integer> solution(int[] arr1, int[] arr2){
  ArrayList<Integer> ans = new ArrayList<Integer>();
  
  for(int i : arr1) {
    for(int j : arr2) {
      if(i == j) ans.add(j);
    }
  }
  ans.sort(Comparator.naturalOrder());
  
  return ans;
}
```

1. 이중 `for`문 사용
2. 시간 초과


## 내 코드 - 방법 2:

```java
public ArrayList<Integer> solution(int[] arr1, int[] arr2){
  ArrayList<Integer> arrList1 = new ArrayList<Integer>();
  ArrayList<Integer> arrList2 = new ArrayList<Integer>();
  ArrayList<Integer> ans = new ArrayList<Integer>();
  int idx1 = 0, idx2 = 0;
  
  for(int i : arr1) arrList1.add(i);
  for(int i : arr2) arrList2.add(i);
  
  arrList1.sort(Comparator.naturalOrder());
  arrList2.sort(Comparator.naturalOrder());
  
  while(idx1 < arr1.length && idx2 < arr2.length) {
    if(arrList1.get(idx1) == arrList2.get(idx2)) {
      ans.add(arrList1.get(idx1));
      idx1++;
      idx2++;
    }
    else if(arrList1.get(idx1) > arrList2.get(idx2)) idx2++;
    else if(arrList1.get(idx1) < arrList2.get(idx2)) idx1++;
  }
  
    return ans;
}
```

1. 주어진 배열을 `ArrayList`에 담기
2. `ArrayList` 정렬
3. 두 ArrayList 비교
    - 첫 번째 배열의 값 == 두 번째 배열의 값: 값 추출, 두 배열 인덱스++
    - 첫 번째 배열의 값 > 두 번째 배열의 값: 두 번째 배열 인덱스++
    - 첫 번째 배열의 값 < 두 번째 배열의 값: 첫 번째 배열 인덱스++  


1. 이중 `for`문 사용
2. 시간 초과
- 오히려 더 오래 걸림..
- 정렬을 너무 많이 쓴 듯 하다
  
## 내 코드 - 방법 3:

```java
public ArrayList<Integer> solution(int[] arr1, int[] arr2){
  ArrayList<Integer> merge = new ArrayList<Integer>();
  int[] tmpArr = new int[arr1.length + arr2.length];
  ArrayList<Integer> ans = new ArrayList<Integer>();
  
  for(int i : arr1) merge.add(i);
  for(int i : arr2) merge.add(i);
  
  merge.sort(Comparator.naturalOrder());
  for(int i = 0; i < merge.size(); i++) {
    tmpArr[i] = merge.get(i);
  }
  
  for(int i = 0; i < tmpArr.length - 1; i++) {
    if(tmpArr[i] == tmpArr[i + 1]) ans.add(tmpArr[i]);
  }
  
  return ans;
}
```

1. `ArrayList` 생성
2. `ArrayList`에 배열 2개 합치기
3. 정렬
4. 각 집합의 원소는 중복되지 않기 때문에 `ArratList` 탐색하면서 중복되는 값 탐색 후 반환

## 정답

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조


```
- 

## 개선된 점:


끝!
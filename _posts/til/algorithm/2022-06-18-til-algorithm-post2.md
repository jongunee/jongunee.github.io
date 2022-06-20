---
layout: post
title: Array \#8. 등수구하기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#8. 등수구하기

* toc
{:toc .large-only}

## 문제: 

> 입력된 N명의 학생 점수에 따라 등수 순서대로 출력  
> ※같은 점수가 입력될 경우 높은 등수로 동일 처리


## 내 코드:

```java
public String solution(int[] arr){
  String ans = "";
  Map<Integer, Integer> map= new HashMap<Integer, Integer>();
  for(int i = 0; i < arr.length; i++) {
    map.put(i, arr[i]);
  }
  List<Entry<Integer, Integer>> list = new ArrayList<>(map.entrySet());
  list.sort(Entry.comparingByValue(Collections.reverseOrder()));
  
  int tmp = list.get(0).getValue();
  map.replace(list.get(0).getKey(), 1);
  for(int i = 1; i < list.size(); i++) {
    if(list.get(i).getValue() == tmp) {
      tmp = list.get(i).getValue();
      map.replace(list.get(i).getKey(), list.get(i-1).getValue());
    }
    else {
      tmp = list.get(i).getValue();
      map.replace(list.get(i).getKey(), i+1);
    }
  }
  
  list.sort(Entry.comparingByKey());
  
  for(int i = 0; i < list.size(); i++) {
    ans = ans + map.get(i) + " ";
  }
  
  return ans;
}
```

1. 학생의 인덱스를 기억하기 위해 `Map` 사용
    - `Key`: 인덱스
    - `Value`: 점수
2. `Map`의 `value`(점수)에 따라 내림차순 정렬
3. `value`에 점수 대신 등수 대입
    - 다르면 현 인덱스 + 1 대입 (인덱스는 0부터 시작)
    - 같으면 이전 인덱스의 등수 대입
4. `Key`값에 따라 오름차순 정렬로 출력


## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int[] solution(int n, int[] arr){
  int cnt = 1;
  int[] ans = new int[n];
  for(int i = 0; i < arr.length; i++) {
    for(int j = 0; j < arr.length; j++) {
      if(arr[i] < arr[j]) cnt++;
    }
    ans[i] = cnt;
    cnt = 1;
  }
  
  return ans;
}
```
- `cnt` 1로 시작
- 이중 `for`문을 돌면서 현재 값이 만나는 값보다 작으면 등수롤 1씩 더함


## 개선된 점:
- 말도 안되게 어렵게 풀었다...
- 이중 `for`문을 돌리면 시간 초과일거라고 착각.. 배열의 최대 크기가 100인 것을 보지 못했다
- 다시 한 번 생각해보자


끝!
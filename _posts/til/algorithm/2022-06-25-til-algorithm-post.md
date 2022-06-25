---
layout: post
title: Map/Set \#1. 학급회장
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Map/Set \#1. 학급회장

* toc
{:toc .large-only}

## 문제: 

> 학급 회장을 뽑는 투표를 할 때 학생수 N과 투표결과가 문자열이 주어진다. 가장 많은 표를 받은 후보가 학급 회장이 될 때 학급 회장이 된 후보 반환  
> ※ 최다득표수가 동일한 후보가 여러명인 경우는 없음 


## 내 코드:

```java
public char solution(int n, String vote){
  char ans = ' ';
  int max = 0;
  Map<Character, Integer> map = new HashMap<Character, Integer>();
  
  for(char c: vote.toCharArray()) {
    if(map.get(c) == null) map.put(c, 1);
    map.put(c, map.get(c) + 1);
  }
  Set<Character> keySet = map.keySet();
  for(char c : keySet) {
    if(map.get(c) > max) {
      max = map.get(c);
      ans = c;
    }
  }

  return ans;
}
```

1. `map` 생성
    - `Key`: 후보명
    - `Value`: 득표수
2. `for`문을 돌면서 `map`에 문자 개수 카운트해서 넣기
3. `Set`을 생성해서 후보명 keySet에 담기
4. 가장 많은 득표수를 가진 후보명 반환

## 정답

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public char solution(int n, String vote){
  char ans = ' ';
  int max = 0;
  Map<Character, Integer> map = new HashMap<Character, Integer>();
  
  for(char c: vote.toCharArray()) {
    map.put(c, map.getOrDefault(c, 0) + 1);
  }
  for(char c : map.keySet()) {
    if(map.get(c) > max) {
      max = map.get(c);
      ans = c;
    }
  }

  return ans;
}
```

- 방식은 내 풀이와 동일
- `map.getOrDefault()`: value가 비어있으면 default 값을 넣어줌
- `Set` 담는 부분 생략

## HashMap 주요 메서드

| 메서드 | 설명 |
| --- | --- |
| `get(key)` | `key` 값에 매핑되는 `value` 값 반환 |
| `getOrDefault(key, default value)` | `get`과 동일. 만약 비어있다면 default value 값을 넣어줌 |
| `containsKey(key)` | `map`에 key가 들어있면 `true` 없으면 `false` 반환 |
| `size()` | `key`의 종류 개수 반환 |
| `remove(key)` | `key` 삭제 후 해당 `value` 반환 |
| `keySet` | `map`에 담긴 `key`를 `set`에 담아 반환 |


## 개선된 점:

- `map.getOrDefault()` 메서드로 코드를 간소화 시킬 수 있다
- 굳이 `Set`을 생성해서 다시 담을 필요는 없었다

끝!
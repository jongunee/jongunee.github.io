---
layout: post
title: Map/Set \#4. 모든 아나그램 찾기
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Map/Set \#4. 모든 아나그램 찾기

* toc
{:toc .large-only}

## 문제: 

> S문자열에서 T문자열과 아나그램이 되는 S의 부분문자열 개수 반환
> ※ 부분문자열은 연속된 문자열이어야 한다


## 내 코드:

```java
public int solution(String s, String t){
  int ans = 0, first = 0, cnt = 0;
  Map<Character, Integer> sMap = new HashMap<Character, Integer>();
  Map<Character, Integer> tMap = new HashMap<Character, Integer>();
  
  for(int i = 0; i < t.length() -1; i++) {
    sMap.put(s.charAt(i), sMap.getOrDefault(s.charAt(i), 0) + 1);
  }
  for(char c : t.toCharArray()) {
    tMap.put(c, tMap.getOrDefault(c, 0) + 1);
  }
  
  for(int i = t.length() - 1; i < s.length(); i++) {
    sMap.put(s.charAt(i), sMap.getOrDefault(s.charAt(i), 0) + 1);
    for(char c : t.toCharArray()) {
      if(sMap.get(c) != tMap.get(c)) break;
      else {
        cnt++;
      }
    }
    if(cnt == t.length()) ans++;
    cnt = 0;

    sMap.put(s.charAt(first), sMap.get(s.charAt(first)) - 1);
    first++;
    
  }
  
  return ans;
}
```

1. `Map` 2개 생성
2. `Map` 세팅
    - `s` 문자열 구간에서 `t` 문자열 개수-1 만큼  `sMap`에 카운트
    - 문자열 `t`의 문자 개수 `tMap`에 카운트
3. 구간마다
    - 그 다음에 들어올 문자 `Map`에 추가해 개수 카운트
    - `sMap`과 `tMap`의 문자 개수 동일한지 확인해서 동일하면 답 카운트
    - 구간의 맨 앞 문자 및 카운트 개수 제거



## 정답:

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public int solution(String s, String t){
  int ans = 0, first = 0;
  Map<Character, Integer> sMap = new HashMap<Character, Integer>();
  Map<Character, Integer> tMap = new HashMap<Character, Integer>();
  
  for(int i = 0; i < t.length() -1; i++) {
    sMap.put(s.charAt(i), sMap.getOrDefault(s.charAt(i), 0) + 1);
  }
  for(char c : t.toCharArray()) {
    tMap.put(c, tMap.getOrDefault(c, 0) + 1);
  }
  
  for(int i = t.length() - 1; i < s.length(); i++) {
    sMap.put(s.charAt(i), sMap.getOrDefault(s.charAt(i), 0) + 1);
    if(sMap.equals(tMap)) ans++;

    sMap.put(s.charAt(first), sMap.get(s.charAt(first)) - 1);
    if(sMap.get(s.charAt(first)) == 0) sMap.remove(s.charAt(first));
    first++;
  }
  
  return ans;
}
```

- 방식은 동일
- `sMap`과 `tMap`을 비교할 떄 `Map.equals()` 사용해서 비교  


## 개선된 점:

- `Map.equals()` 메서드를 몰랐다 
- `sMap`과 `tMap`을 비교할 떄 비교할 때 `for`문을 돌려서 요소를 하나하나 비교했는데 `Map.equals()`로 간단하게 구할 수 있었다
- 기억하자 `Map.equals()` ...

끝!
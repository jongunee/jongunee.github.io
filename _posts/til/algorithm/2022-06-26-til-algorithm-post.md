---
layout: post
title: Map/Set \#2. 아나그램
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Map/Set \#2. 아나그램

* toc
{:toc .large-only}

## 문제: 

> 아나그램이란 두 문자열의 알파벳 나열 순서는 다르지만 구성이 일치한 것을 말한다. 주어진 두 문자열이 아나그램이면 "YES", 아니면 "NO" 반환  
> ※ 알파벳 개수도 동일해야 하며 대소문자를 구분한다 


## 내 코드:

```java
public String solution(String str1, String str2){
  String ans = "NO";
  
  Map<Character, Integer> map1 = new HashMap<Character, Integer>();
  Map<Character, Integer> map2 = new HashMap<Character, Integer>();
  
  if(str1.length() != str2.length()) return ans;
  
  for(char c: str1.toCharArray()) {
    if(map1.get(c) == null) map1.put(c, 1);
    map1.put(c, map1.get(c) + 1);
  }
  for(char c: str2.toCharArray()) {
    if(map2.get(c) == null) map2.put(c, 1);
    map2.put(c, map2.get(c) + 1);
  }

	for(char c : map1.keySet()) {
    if(map1.get(c) != map2.get(c)) return ans;
  }
  
  ans = "YES";
  
  return ans;
}
```

1. `Map` 2개 생성
2. 각 `Map`에 각 문자열의 개수 카운트
3. 각 `Map`에 담겨있는 알파벳 개수를 비교해서 결과 반환


## 정답:

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

public String solution(String str1, String str2){
  String ans = "YES";
  
  Map<Character, Integer> map = new HashMap<Character, Integer>();
  
  for(char c : str1.toCharArray()) {
    map.put(c, map.getOrDefault(c, 0) + 1);
  }
  for(char c : str2.toCharArray()) {
    if(!map.containsKey(c) || map.get(c) == 0) return "NO";
    map.put(c, map.get(c) - 1);
  }
  
    return ans;
}
```

1. `Map` 1개 생성
2. 첫 번째 문자열의 알파벳 개수 카운트
3. 두 번째 문자열을 탐색
    - 첫 번째 문자열에 없는 알파벳 발견시 "NO" 반환
    - 첫 번째 문자열 중 알파벳 개수가 안맞는 경우 "NO" 반환
    - 모두 해당 안되면 `value` 값 -1
4. 중간에 `rerturn` 되지 않으면 "YES" 반환


## 개선된 점:

- `for`문을 3번 돌렸는데 2번 만에 끝낼 수 있었다
- 이런 방법도 있구나~ 

끝!
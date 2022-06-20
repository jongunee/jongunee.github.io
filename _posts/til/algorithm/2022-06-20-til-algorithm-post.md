---
layout: post
title: Array \#10. 봉우리
description: >
    인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - algorithm
---

# Array \#10. 봉우리

* toc
{:toc .large-only}

## 문제: 

> 봉우리의 개수 구하기  
>N * N 격자판이 주어지고 각 격자에 지역의 높이가 쓰여져 있다. 격자판의 숫자 중 상화좌우에 위치한 숫자보다 크면 봉우리 지역으로 판단한다.  
> ※ 격자 가장자리는 0


## 내 코드:

```java
public boolean isHighest(int[][] arr, int x, int y) {
  if(arr[x][y] <= arr[x][y - 1]) return false;
  else if(arr[x][y] <= arr[x][y + 1]) return false;
  else if(arr[x][y] <= arr[x - 1][y]) return false;
  else if(arr[x][y] <= arr[x + 1][y]) return false;
  return true;
}

public int solution(int n, int[][] arr){
  int ans = 0;
  int[][] area = new int[n + 2][n + 2];
  
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      area[i + 1][j + 1] = arr[i][j];
    }
  }
  for(int i = 1; i < n + 1; i++) {
    for(int j = 1; j < n + 1; j++) {
      if(isHighest(area, i, j)) ans++;
    }
  }
  
    return ans;
}
```

1. 봉우리인지 확인하는 함수 구현
    - 격자판의 한 좌표에서 상화좌우 숫자를 확인
2. 가장자리 제외한 좌표에서 위 함수 실행
3. 봉우리 갯수 반환

## 다른 방법 

```java
인프런 자바(Java) 알고리즘 문제풀이 : 코딩테스트 대비 답 참조

int[] dx = {-1, 0, 1, 0};
int[] dy = {0, 1, 0, -1};
public int solution(int n, int[][] arr){
  int ans = 0;
  for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
      boolean flag = true;
      for(int k =0; k < 4; k++) {
        int nx = i + dx[k];
        int ny = j + dy[k];
        if(nx >= 0 && nx < n && ny >= 0 && ny < n && arr[nx][ny] >= arr[i][j]) {
          flag = false;
          break;
        }
      }
      if(flag) ans++;
    }
  }
  
    return ans;
}
```
- 상하좌우 연산을 위한 dx, dy 배열 생성
- `if`문 대신 `for`문을 통해 확인



## 개선된 점:
- 내 코드는 상화좌우만 판단해서 `if`문이 4개였지만 대각선까지 확인한다면 `if`문이 8개 였을 것 ➡️비효율적
- 다음 번엔 dx, dy 방식으로 구해봐야겠다.

끝!
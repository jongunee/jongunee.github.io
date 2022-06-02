---
layout: post
title: 컬렉션 프레임워크
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

# 배열, 링크드리스트, 스택, 큐, 트리, 해싱

* toc
{:toc .large-only}

### 배열(Array)
> 같은 형의 데이터 타입을 메모리에 저장하는 <span style='background-color: #f5f0ff'>선형적</span> 자료구조로 눈리적 구조와 물리적 구조가 <span style='background-color: #f5f0ff'>동일</span>하다.
- fixed length
- 인덱스 연산
- n개에 종속적 - 중간 <span style='background-color: #f5f0ff'>데이터 삽입/삭제</span>가 어려움
- 데이터가 가변적이지 않을 경우 유리
- ex) `ArrayList`, `Vector`

---

### 링크드리스트(LinkedList)
>각 노드가 데이터와 포인터를 가지고 있는 형태 
- 배열의 단점을 보완
- 중간 <span style='background-color: #f5f0ff'>데이터 삽입/삭제</span>에 편리
- 유기적이고 가변적인 배열이 유리

---

### 스택(Stack)
- 선형 자료구조
- LIFO 구조 - 나중에 들어온 데이터가 먼저 나가는 구조
- `ArrayList`에서 `add()`로 삽입, `remove(size() - 1)`로 제거하면 구현 가능
- `Peek()`은 스택 맨 위 원소 반환

---

### 큐(Queue)
- 대기열 구조
- FIFO 구조 - 먼저 들어온 데이터가 먼저 나가는 구조
- `ArrayList`에서 `add()`로 삽입, `remove(0)`로 제거하면 구현 가능
- Call 처리할 떄 사용

---

### 트리(Tree)
- 자료의 검색에 사용
- 선형 구조는 아니고 계층이 있는 구조
- Key 값의 중복을 허용하지 않음
- 최대 복잡도 - $O(log_{2}{n})$
- ex) Binary Search Tree, AVL Tree, Red-Black Tree 등이 있음

---

### 해싱(Hashing)
- 산술 연산을 이용한 검색 방식
- Key-Value, Hash function으로 구현
- Collision이 생길 수 있기 때문에 해결할 수 있도록 구현되어야 함
- n개일 떄 복잡도 - $O(1)$
- ex) `HashMap`, `HashSet`

끝!
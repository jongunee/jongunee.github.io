---
layout: post
title: CRUD
description: >
  생활 코딩 데이터베이스 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - db
---

## CRUD

* toc
{:toc .large-only}

## 테이블 구조 확인
```sql
DESC topic;
```

## CREATE - 행 추가하기

```sql
INSERT INTO topic (title, description, created, author, profile) VALUES('MySQL', 'MySQL is ...', NOW(), 'PJW', 'developer');
```

## READ - 데이터 가져오기

topic 테이블의 모든 데이터 가져오기

```sql
SELECT * FROM topic;
```

원하는 항목의 데이터만 가져오기

```sql
SELECT id, title, created, author FROM topic;
```

author가 'PJW'인 데이터만 출력
```sql
SELECT id, title, created, author FROM topic WHERE author = 'PJW';
```

내림차순 정렬해서 출력
```sql
SELECT id, title, created, author FROM topic WHERE author = 'PJW' ORDER BY id DESC;
```

출력 개수 2개로 제한 - 데이터 10억개 모두 가져오려고 한다면 에러 발생할 것 
```sql
SELECT id, title, created, author FROM topic WHERE author = 'PJW' ORDER BY id DESC LIMIT 2;
```

## UPDATE - 수정하기
author 'egoing'에서 'PJW'로 수정
```sql
UPDATE topic SET author = 'PJW', title='MongoDB' WHERE id = 5;
```

## DELETE - 삭제하기
id가 5인 행 삭제
```sql
DELETE FROM topic where id = 5;
```


끝!
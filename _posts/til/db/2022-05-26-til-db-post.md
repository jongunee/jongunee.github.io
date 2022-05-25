---
layout: post
title: MySQL 테이블
description: >
  생활 코딩 데이터베이스 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - db
---

## MySQL 테이블

* toc
{:toc .large-only}

## MySQL 테이블 생성

__명령어__

```sql
데이터베이스에 접근
```sql
use opentutorials;
```

테이블 생성
```sql
CREATE TABLE topic(
  id INT(11) NOT NULL AUTO_INCREMENT,
  title VARCHAR(100) NOT NULL,
  description TEXT NULL,
  created DATETIME NOT NULL,
  author VARCHAR(15) NULL,
  profile VARCHAR(200) NULL,
  PRIMARY KEY(id)
);
```


데이터베이스에 접근
```sql
use opentutorials;
```

테이블 생성
```sql
CREATE TABLE topic(
```

크기 11인 Int 타입으로 설정, NULL이 될 수 없고 행이 추가될 떄 자동으로 전의 ID보다 1 증가 시킨다.
```sql
id INT(11) NOT NULL AUTO_INCREMENT,
```

가변 문자열 타입 설정, 크기가 100이기 때문에 100이 넘어가면 짤린다는 의미
```sql
title VARCHAR(100) NOT NULL,
```

Text 타입 설정
```sql
description TEXT NULL,
```

날짜 타입 지정
```sql
created DATETIME NOT NULL,
```

가변 문자열 타입 설정, 이름은 익명일 수 있기 떄문에 NULL
```sql
author VARCHAR(15) NULL,
```

가변 문자열 타입 설정, 내용이 없을 수 있기 때문에 NULL
```sql
profile VARCHAR(200) NULL,
```

Primary 키 설정 - 고유하며 중복 방지
```sql
PRIMARY KEY(id)
```

괄호 닫기
```sql
);
```

## 테이블 삭제
```sql
DROP TABLE topic;
```

## 테이블 확인
```sql
SHOW TABLES;
```
## 테이블 이름 수정
```sql
RENAME TABLE topic TO topic_backup;
```

## 비밀번호 설정
```sql
SET PASSWORD = PASSWORD('MY PASSWORD');
```

결과

| id | title | description | created | author | profile |
| --- | --- | --- | --- | --- | --- |
| 1 | MySQL | MySQL is ... | 2022-01-01 | PJW | developer|



끝!
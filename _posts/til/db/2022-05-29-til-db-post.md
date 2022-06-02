---
layout: post
title: MySQL 클라이언트
description: >
  생활 코딩 데이터베이스 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - db
---

# MySQL 클라이언트

* toc
{:toc .large-only}

## MySQL Monitor

- CLI 명령어 기반
- 어디서든 실행 가능
- 명령어를 기억해야 한다는 단점

다른 IP 접속시
```sql
mysql -urrot -p -h127.0.0.1
```

## MySQL Workbench
- connection에 127.0.0.1 입력해서 MySQL 서버로 접속
- Schemas에서 DB에 접근 
- Query에서 번개 모양 실행버튼으로 명령어 실행
- Workbench 클라이언트도 명령어를 전송해서 쿼리 기능이 실행 됨

## SQL 백업
- mysqldump
- binary log

## 클라우드
- AWS RDS
- Google Cloud SQL for MySQL
- AZURE Database for MySQL

## DB 시스템 핸들링을 위한 프로그래밍 키워드
- Phython mySQL api
- PHP mysql api
- JAVA mysql api

끝!
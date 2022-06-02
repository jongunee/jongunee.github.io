---
layout: post
title: 관계형 데이터베이스
description: >
  생활 코딩 데이터베이스 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - db
---

# 관계형 데이터베이스

* toc
{:toc .large-only}

## 관계형 데이터베이스의 필요성
- 중복된 데이터가 반복된다는 것은 개선의 여지가 필요하다는 것
- 굉장히 큰 데이터가 1억 번 반복된다면 큰 경제적 손실
- 수정시 반복되는 모든 데이터를 수정해야 함
- 중복 방지

__기존 topic 테이블__
| id | title | description | created | author | profile |
| --- | --- | --- | --- | --- | --- |
| 1 | MySQL | MySQL is ... | 2022-01-01 | PJW | developer |
| 2 | ORACLE | ORACLE is ... | 2022-01-02 | PJW | developer |
| 3 | SQL Server | SQL Server is ... | 2022-01-03 | duru | db administrator |
| 4 | PostgreSQL | PostgreSQL is ... | 2022-01-04 | taeho | data scientist, developer |
| 5 | MongoDB | MongoDB is ... | 2022-01-05 | PJW | developer |

---

__author 테이블 추가__
```sql
CREATE TABLE `author` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `profile` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
```

```sql
INSERT INTO `author` VALUES (1,'PJW','developer');
INSERT INTO `author` VALUES (2,'duru','database administrator');
INSERT INTO `author` VALUES (3,'taeho','data scientist, developer');
```

__author 테이블 결과__
| id | name | profile |
| --- | --- | --- |
| 1 | PJW | developer |
| 2 | duru | db administrator |
| 3 | taeho | data scientist, developer |

---

__새로운 topic 테이블 추가__
```sql
CREATE TABLE `topic` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(30) NOT NULL,
  `description` text,
  `created` datetime NOT NULL,
  `author_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
```
```sql
INSERT INTO `topic` VALUES (1,'MySQL','MySQL is...',NOW(),1);
INSERT INTO `topic` VALUES (2,'Oracle','Oracle is ...',NOW(),1);
INSERT INTO `topic` VALUES (3,'SQL Server','SQL Server is ...',NOW(),2);
INSERT INTO `topic` VALUES (4,'PostgreSQL','PostgreSQL is ...',NOW(),3);
INSERT INTO `topic` VALUES (5,'MongoDB','MongoDB is ...',NOW(),1);
```

__새로운 topic 테이블 결과__
| id | title | description | created | author_id |
| --- | --- | --- | --- | --- |
| 1 | MySQL | MySQL is ... | 2022-01-01 | 1 |
| 2 | ORACLE | ORACLE is ... | 2022-01-02 | 1 |
| 3 | SQL Server | SQL Server is ... | 2022-01-03 | 2 |
| 4 | PostgreSQL | PostgreSQL is ... | 2022-01-04 | 3 |
| 5 | MongoDB | MongoDB is ... | 2022-01-05 | 1 |

---

## Join
topic 테이블의 authour_id와 author 테이블의 id가 같은 경우 Join해서 보여주기
```sql
SELECT * FROM topic LEFT JOIN author ON topic.author_id = author.id;
```

__Join 결과__
| id | title      | description       | created             | author_id | id   | name  | profile                   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  1 | MySQL      | MySQL is...       | 2022-05-19 14:45:24 |         1 |    1 | PJW   | developer                 |
|  2 | Oracle     | Oracle is ...     | 2022-05-19 14:45:24 |         1 |    1 | PJW   | developer                 |
|  3 | SQL Server | SQL Server is ... | 2022-05-19 14:45:24 |         2 |    2 | duru  | database administrator    |
|  4 | PostgreSQL | PostgreSQL is ... | 2022-05-19 14:45:24 |         3 |    3 | taeho | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2022-05-19 14:45:26 |         1 |    1 | PJW   | developer                 |

author_id와 id 컬럼 제외하고 보여주기
```sql
SELECT topic.id, title, description, created, name, profile FROM topic LEFT JOIN author ON topic.author_id = author.id;
```

출력할 때 id를 author_id로 바꿔서 보여주기
```sql
SELECT topic.id AS topic_id, title, description, created, name, profile FROM topic LEFT JOIN author ON topic.author_id = author.id;
```



끝!
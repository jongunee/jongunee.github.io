---
layout: post
title: MySQL 구조
description: >
  생활 코딩 데이터베이스 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - db
---

## MySQL 구조

* toc
{:toc .large-only}

## 표(Table)
> 관계형 DBMS는 Excel의 스프테드시트와 비슷한 구조를 가지기 떄문에 데이터를 표에 저장

## 데이터베이스(Database)/스키마
> 표가 늘어나게 되면 정리가 필요해진다. 연관된 표를 그룹핑할 수 있는 것이  <span style='background-color: #f5f0ff'>DB</span>.  
> <span style='background-color: #f5f0ff'>스키마</span>는 표들을 서로 그룹핑 할 때 사용하는 일종의 폴더라고 생각하면 된다. 

## 데이터베이스 서버
> DB가 저장되는 공간  

표 $\subset$ DB/스키마 $\subset$ DB 서버    

## Root 권한

유저(root)/패스워드로 접속 명령어
```SQL
bin ./mysql -uroot -p
```
root도 유저 중 하나이지만 관리자이기 때문에 모든 권한을 가진다. 루트 권한으로 직접 DB를 다루는 것은 매우 위험하기 때문에 별도 유저를 생성해서 다루는 것이 좋다.

끝!
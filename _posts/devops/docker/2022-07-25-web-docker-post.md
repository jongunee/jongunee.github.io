---
layout: post
title: 도커 컴포즈
description: >
  인프런 - 초보를 위한 도커 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - devops
  - docker
---

# 도커 컴포즈

* toc
{:toc .large-only}

## 도커 컴포즈 설치

리눅스 환경에서 설치

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

권한 주기
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

## 도커 컴포즈 예시

```yml
version: '2'
services: #실행할 컨테이너 정의
  db:
    image: mysql:5.7  #이미지 이름
    volumes:  #마운트 디렉토리(호스트 디렉토리:컨테이너 디렉토리)
      - ./mysql:/var/lib/mysql
    restart: always #재시작 정책
    environment: #컨테이너 환경변수
      MYSQL_ROOT_PASSWORD: wordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
  wordpress:
    image: wordpress:latest
    volumes:
      - ./wp:/var/www/html
    ports:
      - "8000:80" #호스트 포트:컨테이너 포트
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_PASSWORD: wordpress
```

## 실행 및 종료

도커 컴포즈 실행

```docker
docker-compose up -d
```

도커 컴포즈 종료

```docker
docker-compose down
```

도커 컴포즈를 이용하면 한 번에 여러 개의 컨테이너 시스템을 구성할 때 편리하며 명령어를 yml파일로 정리해서 실행시킬 수 있다

## 도커 컴포즈 문법

- __version:__ yml 파일 명세 버전
- __services:__ 실행할 컨테이너 정의
- __image:__ 컨테이너에 사용할 이미지 이름과 태그
- __ports:__ 컨테이너와 연결할 포트  
{호스트 포트}:{컨테이너 포트}
- __environment:__ 컨테이너 환경변수
- __volumes:__ 마운트하려는 디렉토리  
{호스트 디렉토리}:{컨테이너 디렉토리}
- __restart:__ 재시작 정책
- __build:__ 이미지 자체 빌드로 별도 도커 파일 필요


## 도커 컴포즈 명령어

- __up:__ docker-compose.yml에 정의된 컨테이너 실행
  - __--force-recreate:__ 컨테이너 새로 만들기
  - __--build:__ 도커 이미지 다시 빌드
- __start:__ 멈춘 컨테이너 재개
- __restart:__ 컨테이너 재시작
- __stop:__ 컨테이너 중지
- __down:__ 컨테이너 종료 후 삭제
- __logs:__ 컨테이너 로그 follow
- __ps:__ 컨테이너 목록
- __exec:__ 실행 중인 컨테이너에서 명령어 실행
- __build:__ 컨테이너 build 부분에 정의된 내용대로 빌드




<span style="font-size:70%">[참조] 인프런 - 초보를 위한 도커 안내서

끝!

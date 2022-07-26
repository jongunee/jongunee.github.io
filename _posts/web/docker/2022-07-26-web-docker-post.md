---
layout: post
title: 도커 이미지
description: >
  인프런 - 초보를 위한 도커 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 도커 이미지

* toc
{:toc .large-only}

## 도커 이미지란?

> 프로세스가 실행되는 파일들의 집합 또는 환경

이미지에는 두 가지 종류의 이미지가 있다
- Only Read: 읽기 전용
- Writable: 쓰기 가능

## 이미지 생성

__이미지 과정__

![그림1](/assets/img/docker/custom%20image.png)

- 기본 이미지를 불러오기
- 실행시킨 컨테이너에서 수정
- 수정 사항을 새로운 이미지로 저장
- 새로운 Custom Image를 불러와 컨테이너로 실행시키면 수정 사항이 적용됨

__빌드__

```docker
docker build -t name/ubuntu:git01 .
docker build -t 이름/이미지이름:태그 .
```

__.dockerignore__

- .gitgnore과 비슷한 역할
- 도커 빌드 컨텍스트에서 지정된 패턴의 파일을 무시

__Dockerfile 명령어__

| __명령어__ | __설명__ |
| --- | --- |
| __FROM__ | 기본 이미지 |
| __RUN__ | 쉘 명령어 실행 |
| __CMD__ | 컨테이너 기본 실행 명령어 (Entrypoint) |
| __EXPOSE__ | 오픈되는 포트 정보 |
| __ENV__ | 환경변수 설정 |
| __ADD__ | 파일 또는 디렉토리 추가. URL/ZIP 사용 가능 |
| __COPY__ | 파일 또는 디렉토리 추가 |
| __ENTRYPOINT__ | 컨테이너 기본 실행 명령어 |
| __VOLUME__ | 외부 마운트 포인트 생성 |
| __USER__ | RUN, CMD, ENTRYPOINT를 실행하는 사용자 |
| __WORKDIR__ | 작업 디렉토리 설정 |
| __ARGS__ | 빌드타임 환경변수 설정 |
| __LABEL__ | key-value 데이터 |
| __ONBUILD__ | 다른 빌드의 베이스로 사용될때 사용하는 명령어 |

__Dockerfile 예시__

```dockerfile
FROM ununtu:latest
RUN apt-get update
RUN apt-get install -y git
```

이미지를 생성해서 컨테이너를 실행시키고 명령어를 입력하고 다시 이미지로 생성하는 작업을 <span style='background-color: #f5f0ff'>Dockerfile</span>을 이용해 바로 이미지를 생성할 수 있다




<span style="font-size:70%">[참조] 인프런 - 초보를 위한 도커 안내서

끝!

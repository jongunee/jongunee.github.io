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

## 웹서버 이미지 생성하기

__서버__

app.js에 서버 코드 작성

```js
// Require the framework and instantiate it
const fastify = require('fastify')({
  logger: true
})

// Declare a route
fastify.get('/', function (request, reply) {
  reply.send({ hello: 'world' })
})

// Run the server!
fastify.listen(3000, '0.0.0.0', function (err, address) {
  if (err) {
    fastify.log.error(err)
    process.exit(1)
  }
  fastify.log.info(`server listening on ${address}`)
})
```

__도커 파일 작성__
```dockerfile
# 1. node 설치
FROM    ubuntu:22.04
RUN     apt-get update
RUN     DEBIAN_FRONTEND=noninteractive apt-get install -y curl
RUN     curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN     DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs

# 2. 소스 복사
COPY    . /usr/src/app

# 3. Nodejs 패키지 설치
WORKDIR /usr/src/app
RUN     npm install

# 4. WEB 서버 실행 (Listen 포트 정의)
EXPOSE 3000
CMD    node app.js
```

__도커 파일 - 최적화__
```dockerfile
# 1. node 이미지 사용
FROM    node:16-alpine

# 2. 패키지 우선 복사
COPY    ./package* /usr/src/app/
WORKDIR /usr/src/app
RUN     npm install

# 3. 소스 복사
COPY . /usr/src/app

# 4. WEB 서버 실행 (Listen 포트 정의)
EXPOSE 3000
CMD    node app.js
```
node:16-alpine ➡️우분투 이미지를 가져와 node를 설치할 수도 있지만 불필요한 것들을 제외하고 node만 설치되어 있는 이미지를 가져와 메모리를 줄일 수 있다

__.dockerignore__

```docker
node_modules/*
```
node_modules 파일까지 올릴 필요는 없기 때문에 이미지 생성에서 제외한다

__빌드 및 실행__

```docker
docker build -t web
```

```docker
docker run --rm -d -p 3000:3000 web
```

## 도커파일 문법

### FROM
- 베이스 이미지 지정

```docker
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

### COPY
- 파일 또는 디렉토리 추가
```docker
COPY [--chown=<user>:<group>] <src>... <dest>
```

### RUN
- 명령어 실행

```docker
RUN <command>
```

### WORKDIR
- 작업 디렉토리 변경

```docker
WORKDIR /path/to/workdir
```

### EXPOSE
- 컨테이너에서 사용하는 포트 정보

```docker
WORKDIR /path/to/workdir
```

### CMD
- 컨테이너 생성시 실행할 명령어

```docker
CMD ["executable","param1","param2"]
CMD command param1 param2
```

## 이미지 저장

도커 로그인

```docker
docker login
```

태그 생성

```docker
docker tag {이미지명} {id}/{이미지명}:{tag명}
```

이미지 업로드

```docker
docker push {id}/{이미지명}:{tag명}
```

이미지 가져오기

```docker
docker pull {id}/{이미지명}:{tag명}
```

## 이미지 배포

```docker
docker run --rm -d -p 3000:3000 web
```

- dockerhub에 이미지를 올리는 순간 어떤 서버에서든 컨테이너를 실행할 수 있다
- 자연스레 배포가 완성되는 것
- node 설치, 환경 설정 등 필요 없음



<span style="font-size:70%">[참조] 인프런 - 초보를 위한 도커 안내서

끝!

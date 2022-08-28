---
layout: post
title: 도커 설치
description: >
  인프런 - 초보를 위한 도커 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 도커 설치

- toc
  {:toc .large-only}

## 도커 설치

**Windows환경에서 설치하기**

도커는 기본적으로 Linux에서 지워하기 때문에 도커 Windows 버전은 가상머신에 설치됨

**우분트 환경에서 설치하기**

```bash
curl -s https://get.docker.com/ | sudo sh
```

**ubuntu 유저 권한 추가하기**

```bash
sudo usermod -aG docker ubuntu
```

permission denied 오류나면

```bash
sudo chmod 666 /var/run/docker.sock
```

**experimental true로 설정하기**

```bash
sudo nano /etc/docker/daemon.json
```

코드 추가

```
{
  "experimental": true
}
```

도커 엔진 재시작

```bash
sudo systemctl restart docker
```

도커 클라이언트는 도커 호스트에 명령을 전달하고 결과를 받아서 출력하는 구조이다

## 도커 명령어

### run

```bash
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

컨테이너를 실행한다

| 옵션      | 설명                                                   |
| --------- | ------------------------------------------------------ |
| -d        | detached mode (백그라운드 모드)                        |
| -p        | 호스트와 컨테이너의 포트 연결                          |
| -v        | 호스트와 컨테이너의 디렉토리 연결                      |
| -e        | 컨테이너 내에서 사용할 환경변수 설정                   |
| --name    | 컨테이너 이름 설정                                     |
| --rm      | 프로세스 종료시 컨테이너 자동 제거                     |
| -it       | -i 와 -를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
| --network | 네트워크 연결                                          |

- run 명령어를 사용하면 사용할 이미지가 저장되어 있는지 먼저 확인
- 이미지가 없다면 다운로드(pull)하고 컨테이너를 생성(create)하고 시작(start)

**도커로 ubuntu 실행**

도커가 기본적으로 제공하는 이미지로 ubuntu를 실행할 수 있다

```bash
docker run --rm -it ubuntu:20.04 /bin/sh
```

- -it: 키보드 입력
- --rm: 프로세스 종료 후 컨테이너 자동 삭제
- 컨테이너 내부에 들어가 sh 실행
- 현재 가상머신에 설치되어 있는 ubuntu를 실행하는 것이 아니라 도커로 이미지를 받고 컨테이너를 생성시켜 ubuntu를 실행시키는 것

백그라운드에 도커가 실행되고 있지 않다면 상태를 확인하고

```bash
$ sudo systemctl status docker
```

시작해준다

```bash
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

또는

```bash
sudo service docker start
```

### ps 명령어

```bash
docker ps -a
```

실행중인 컨테이너를 확인할 수 있고 `-a` 옵션을 주어 중지된 컨테이너도 확인이 가능하다

### stop 명령어

```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

실행중인 컨테이너를 중지한다

### rm 명령어

```bash
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

종료된 컨테이너를 제거한다

### logs 명령어

```bash
docker logs [OPTIONS] CONTAINER
```

컨테이너의 로그를 확인이 가능하다

- `-f` 옵션을 주면 실시간 로그를 확인할 수 있다

### images 명령어

```bash
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

도커가 다운로드한 이미지 목록을 볼 수 있다

### pull 명령어

```bash
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

이미지를 다운로드 한다. 기본적으로 run 시에 이미지가 없으면 이미지를 자동 다운로드 한다

### rmi 명령어

```bash
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

images 명령어롤 통한 이미지 목록에서 이미지를 삭제한다

### network create 명령어

```bash
docker network create [OPTIONS] NETWORK
docker network create app-network
```

도커 컨테이너끼리 이름으로 통신할 수 있는 가상 네트워크 생성한다

### network connect 명령어

```bash
docker network connect [OPTIONS] NETWORK CONTAINER
docker network connect app-network mysql
```

기존에 생성된 컨테이너에 네트워크를 추가한다

**예시**

```bash
docker run -d -p 8080:80 \
--network=app-network \
-e WORDPRESS_DB_HOST=mysql \
-e WORDPRESS_DB_NAME=wp \
-e WORDPRESS_DB_USER=wp \
-e WORDPRESS_DB_PASSWORD=wp \
wordpress
```

### volume mount (-v) 명령어

```bash
-v /my/own/datadir:/var/lib/mysql
```

컨테이너를 삭제하면 모두 사라지기 때문에 `-v` 옵션을 주어 삭제되면 안되는 데이터를 보관, 불러올 수 있음

**예시**

```bash
docker stop mysql
docker rm mysql
docker run -d -p 3306:3306 \
-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
--network=app-network \
--name mysql \
-v /my/own/datadir:/var/lib/mysql \
mysql:5.7
```

<span style="font-size:70%">[참조] 인프런 - 초보를 위한 도커 안내서

끝!

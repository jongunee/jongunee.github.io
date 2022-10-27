---
layout: post
title: Docker
description: >
  인프런 - 초보를 위한 도커 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - devops
  - docker
---

# 도커

* toc
{:toc .large-only}

## 도커란?

![그림1](/assets/img/docker/docker.JPG)

> 도커란 컨테이너 기반의 가상화 플랫폼으로 이미지를 실행시켜 컨테이너로 생성, 관리가 가능하다

- 다른 라이브러리와의 충돌 방지
- 격리된 환경 적용 가능
- 어떤 환경이라도 동일한 조건에서 실행이 가능
- 일관된 결과 보장

__이전 방식__

1. 문서 매뉴얼
2. 상태 관리 도구
    - 어렵고 서버에 다른 버전의 프로그램을 여러개 설치할 시 문제 발생
    - ex) Puppet, Chef
3. 가상 머신
    - 한 서버에 여러개 설치하기 쉬움
    - 용량이 커서 이미지 공유가 어렵고 느림
4. 리눅스 기능을 이용한 자원 격리
    - 파일, 디렉토리를 가상으로 분리
    - CPU, 메모리, I/O 그룹별로 제한
    - 기술을 적용하기 어려움

__도커 등장__

- 컨테이너: 격리된 환경에서 작동하는 프로세스
- 리눅스 커널 여러 기술 활용
- 가상머신 기술보다 가벼움
- 이미지 단위로 프로세스 실행 환경 구성

## 도커 특징

![그림2](/assets/img/docker/dockerVsVm.JPG)

- VM에 Hypervisor와 Guest OS 부분이 속도 저하의 원인
- 도커는 가상머신이 아니라 격리만 해주는 역할

__확장성/이식성__

- 도커만 설치되어 있다면 어디서든 컨테이너 실행 가능
- 오픈소스이기 때문에 특정 회사/서비스에 종속적이지 않음
- 생성이 간편

__표준성__

- 도커를 사용하지 않으면 서비스별로 배포 방식이 제각각
다름
- 컨테이너라는 표준으로 서버 배포

__이미지__

- Dockerfile이라는 script를 이용하여 이미지에서 컨테이너 생성
- 빌드 서버에서 이미지를 만들면 이미지 저장소에 저장하고 필요시 운영서버에서 불러옴

__설정관리__

- 환경변수로 제어
- 컨테이너를 띄울때 환경변수 같이 지정
  ```sql
  MYSQL_PASS=password
  ```
- 하나의 이미지가 환경변수에 따라 동적으로 설정파일을 생성하도록 만들어져야 함

__자원관리__

- 컨테이너 삭제 후 새로 만들면 모든 데이터 초기화
- 별도 저장소 필요  
  ex)외부 스토리지, AWS - S3
- 세션이나 캐시를 외부로 분리  
  ex) memcached, redis

__강점__

- 클라우드 이미지보다 관리하기 쉬움
- 가상머신처럼 사용하지만 성능저하가 거의 없음
- 간단함
- Git으로 이미지 빌드 기록을 남길 수 있음
- 코드, 설정으로 관리

## 컨테이너

__스케줄링__

> 컨테이너를 적당한 서버에 배포해 주는 작업을 의미한다. 

- 여러 대 서버 중 할일 없는 서버나 순서대로 또는 랜덤하게 배포
- 컨테이너 개수를 여러 개로 늘려 서버가 죽었을 때 실행중이던 컨테이너를 다른 서버에 띄워 주는 것 가능

__클러스터링__

- 여러 개의 서버를 한 곳에서 관리 가능
- 가상 네트워크를 이용하여 분산되어 있는 컨테이너들을 같은 서버에 있는 것처럼 쉽게 통신 가능

__서비스 디스커버리__

- 클러스터 환경에서 컨테이너는 어느 서버에 생성될지 알 수 없기 때문에 서비스를 찾아주는 기능이 필요 

<span style="font-size:70%">[참조] 인프런 - 초보를 위한 도커 안내서

끝!
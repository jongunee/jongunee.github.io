---
layout: post
title: 쿠버네티스 애플리케이션 배포
description: >
  인프런 - 쉽게 시작하는 쿠버네티스(v1.20) 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 쿠버네티스

* toc
{:toc .large-only}

## 쿠버네티스란?

> 도커와 같은 컨테이너 관리용 오케스트레이션으로 가장 빠르게 발전하는 사실상 표준(De facto)  
> Kubernetes는 k8s로 표현하기도 함

구글에서 사용하던 Borg 시스템을 CNCF 재단에 기부해 CNCF가 관리하고 있다 

## 쿠버네티스 필요성

- 도커의 등장으로 이미지화와 컨테이너를 통해 관리가 매우 간편해짐
- 하지만 컨테이너가 늘어나게 된다면 여전히 하나하나 관리하는 것은 어려움
- 로드밸런서로 관리가 가능하지만 자동 처리가 필요
- 쿠버네티스같은 컨테이너 오케스트레이션을 통해 자동화가 가능

__컨테이너 오케스트레이션 특징__

> 컨테이너 오케스트레이션이란 복잡한 컨테이너 환경을 효과적으로 관리하기 위한 도구

- 클러스터
    - 관리가 필요한 노드들이 늘어나면 클러스터 단위로 관리
    - 하나하나 ssh로 접속하는 것은 너무 어렵기 때문에 마스터 서버를 두고 자동 관리하도록 함
    - 클러스트 내 노드들끼리 네트워크 연결하여 통신에 문제가 없도록 함
    - 수 천개 수 만개의 노드가 있더라도 부하 없이 잘 작동하도록 필요
- 상태 관리
    - 특별한 직접적인 조치가 없더라도 상태를 자동으로 맞춰 줌
    - ex) replicas를 2에서 3으로 늘려주면 자동으로 컨테이너가 3개 띄움
- 배포 관리
    - 스케줄링을 통해 부하 조정
    - ex) App을 실행 시킬 때 여유가 있는 서버에서 실행
- 배포 버전관리
    - 노드 하나하나가 아닌 클러스터 단위로 자동 Rollout, Rollback
- 서비스 등록 및 조회
    - 서비스 하나 하나 등록하는 것이 아니라 자동으로 등록 해주고 자동으로 설정 변경, 프로세스 재시작 가능
- 볼륨 스토리지
    - 스토리지 연결을 설정으로 자동화 가능

## 쿠버네티스 마스터

### etcd

- 모든 상태와 데이터 저장
- Key-Vlaue 형태 데이터 저장
- 분산 시스템 구성

### API 서버

- 상태를 바꾸거나 조회
- etcd와 유일하게 통신
- REST API 형태
- 권한 체크 및 요청 차단

### Scheduler

- 새로 생성된 파드 감지 및 실행 노드 선택
- 노드 현재 상태와 파드 요구사항 체크

### Controller

- 끊임없이 상태 체크
- 원하는 상태 유지
- 하나의 프로세스로 실행

### 예시

__조회 흐름__

1. **컨트롤러**가 **API 서버**에 정보 조회  
2. **API 서버**는 **컨트롤러**가 해당 리소스를 볼 수 있는지 정보 조회 권한 체크  
3. 권한이 있다면 **API 서버**가 **etcd**에 정보 조회  
4. **컨트롤러**에 전달

__기본 흐름__

1. **etcd**가 **API 서버**에 원하는 상태 변경됨을 알림
2.  **API 서버**가 **컨트롤러**에 원하는 상태 변경됨을 알림
3. **컨트롤러**가 현재 상태를 원하는 상태로 맞추기 위해 조치를 취해 리소스 변경
4. **컨트롤러**는 리소스 변경 사항을 **API 서버**에 전달
5. **API 서버**는 **컨트롤러**가 정보 갱신 권한이 있는지 체크
6. 권한이 있다면 **API 서버**는  **etcd**에 정보 갱신

## 쿠버네티스 노드

**kubelet**

- 각 노드에서 실행
- 파드 실행/중지
- 파드 상태 체크

**proxy**

- 네트워크 프록시와 부하 분산 역할
- iptables 또는 IPVS를 사용해 설정만 관리


## 쿠버네티스 배포 종류

1. 관리형 쿠버네티스
    - 사용자가 관리할 필요가 없고 필요한 어플리케이션을 올려 배포만 하면됨 
    - ex) aws, Azure, Google Cloud 등
2. 설치형 쿠버네티스
    - 설치를 할 수 있도록 패키지화한 것
    - ex) Rancher, OpenShift
3. 구성형 쿠버네티스
    - 필요로 하는 환경 내에서 자유롭게 구성하고자 하거나 교육이 목적인 경우
    - ex) kuberadm, kops, Kuberspray, KRIB 등



## 개발 환경

1. Vagrant
    - 다운로드: <https://www.vagrantup.com/downloads>
2. Virtual box
    - 다운로드: <https://www.virtualbox.org/>
3. Kubernetes

## 쿠버네티스 실행

- Vagrant 설정해서 한 번에 VM 실행
    ```cmd
    vagrant up
    ```
- 마스터 노드에서 워커 노드 상태 확인
    ```cmd
    kubectl get nodes
    ```

## 애플리케이션 배포

- 마스터 노드에서 워커 노드로 애플리케이션을 설치할 수 있도록 명령
- 애플리케이션 배포 단위는 `Pod(파드)`

__이미지 배포 및 설치__
```cmd
kubectl run nginx --image=nginx
```

__파드 배포 상태 확인__
```cmd
kubectl get pod
```

__배포한 파드 IP 확인__
```cmd
kubectl get pod -o wide
```

Pod(파드)란?
> 파드란 컨테이너의 집합을 의미한다. 하나의 일을 하기 위해 묶여진 컨테이너 집합이지만 대부분 단일 컨테이너가 하나의 파드로 구성된다.

## 배포한 파드 외부 접근

- 특별한 설정 없이 외부에서 배포한 파드에 접근할 수 없음
- 노드 포트에 접근하고 파드의 위치를 찾아가는 구조


__파드 Expose__
```cmd
kubectl expose pod nginx --type=NodePort --port=80
```
__서비스 확인__
```cmd
kubectl get service
```
__결과: 30000 포트 확인__

| NAME | TYPE | CLUSTER-IP | EXTERNAL-IP | PORT(S) | AGE |
| --- | --- | --- | --- | --- | --- |
| nginx | NodePort | \<Cluster IP> | \<none> | 80:<mark>30000</mark>/TCP | 4m32s |

- 노드 정보(IP) 확인
```cmd
kubectl get nodes -o wide
```
__결과__

| NAME | STATUS | ROLES | AGE | VERSION | INTERNAL-IP | EXTERNAL-IP | OS-IMAGE | KERNEL-VERSION | CONTAINER-RUNTIME |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| w1-k8s | Ready | \<none> | 2d17h | v1.20.2 | <mark>192.168.1.101</mark> | \<none> |CentOS | Linux 7 (Core) | \<kernerl v> | docker://19.3.14 |


__외부 접속__ - nginx 확인
```cmd
http://192.168.1.101:30000/
```

하지만 파드가 하나이기 때문에 파드가 죽으면 서비스가 실행이 안됨

## 디플로이먼트를 통한 배포

> 디플로이먼트는 파드를 여러 개 가지고 있는 큰 단위

버전에 따라서 다르지만 파드만 `kubectl run` 명령어를 통해 디플로이먼트는 배포가 불가능하다

```cmd
kubectl run
```

해당 명령어로 파드와 디플로이먼트 모두 배포가 가능하며 특히 `kubectl apply`는 파일을 통해 배포하는 방식이다 

```cmd
kubectl create
```
```cmd
kubectl apply
```

__디플로이먼트 생성__

```cmd
kubectl create deployment deploy-nginx --image=nginx
```
디플로이먼트가 `delploy-nginx`라는 이름의 이미지로 생성됨  

__생성된 결과 확인__
```cmd
kubectl get pods
```

__파드 배포 수 늘리기__
```cmd
kubectl scale deployment deploy-nginx --replicas=3
```

노트포트는 노드 IP를 알아야하는데 로드밸런서는 고유 IP를 만들어 알려줄 수 있어 노드 IP와 포트를 열어주는 부담이 없다

__로드밸런서 배포__

```cmd
kubectl expose deployment deploy-nginx --type=LoadBalancer --port=80
```

서비스가 생성이 되고 확인하면

```cmd
kubectl get services
```
External-IP가 생성되어 접속이 가능하다

__외부 접속__
```cmd
http://192.168.1.11/
```

## 배포 삭제

__서비스 삭제__

```cmd
kubectl delete service nginx
```

__디플로이먼트 삭제__

```cmd
kubectl delete deployment nginx
```

__파드 삭제__

```cmd
kubectl delete pod nginx
```

<span style="font-size:70%">[참조] 인프런 - 쉽게 시작하는 쿠버네티스(v1.20)

끝!

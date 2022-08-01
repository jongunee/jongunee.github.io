---
layout: post
title: 쿠버네티스
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

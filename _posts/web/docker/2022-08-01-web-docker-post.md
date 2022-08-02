---
layout: post
title: 쿠버네티스 구조
description: >
  인프런 - 쉽게 시작하는 쿠버네티스(v1.20) 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 쿠버네티스 구조

* toc
{:toc .large-only}

## 네임스페이스

> 네임스페이스는 한 쿠버네티스 클러스터 내의 논리적 분리 단위를 말한다.

## 쿠버네티스 구조

![그림1](/assets/img/docker/kubernetes_structure.JPG)

- API 서버가 명령을 내리고 다른 etcd, 컨트롤러 매니저 등이 수행할 것 같지만 API 서버는 감시, 상태값 업데이트 역할만 함
- 다른 요소들이 확인하며 상태를 맞춰나가는 형태
- 파드 배포는 실질적으로 워커 노드에서 Kubelet이 함
- 선언적인 구조

__파드 디플로이먼트 형태 배포__

- 레플리카셋은 항상 정해진 개수의 포트가 실행되는 것을 보장
- 파드를 삭제하더라도 정해진 개수만큼 새로운 파드 생성
- 레플리카셋 값을 늘리면 정해진 개수만큼 새로운 파드 생성

## 워커 노드에 문제 생긴 경우

__Kubelet 중지 된 경우__

```cmd
systemctl stop kubelet
```
워커 노드의 kubelet을 중지시키고 배포하게 되면 제대로 배포되지 않고 pending 상태가 됨

```cmd
systemctl start kubelet
```
워커 노드의 kubelet을 다시 시작하고 배포하면 제대로 배포가 된다

__도커(컨테이너 런타임)이 중지 된 경우__

```cmd
systemctl stop docker
```
- 레플리카셋을 중지한 뒤에 배포를 하게 되면 중지된 워커 노드를 제외하고 배포
- 컨테이너 런타임에 문제가 생기면 Kubelet이 이를 인식해서 API 서버에 알림
- 스케줄러가 문제가 생긴 컨테이너에는 배포를 스케줄링 하지 않음
- 균형이 맞지 않는 상태에서 레플리카셋 값을 늘려주게 되면 스케줄러가 값에 따라 균형 맞게 컨테이너에 자동 배포

## 마스터 노드에 문제 생긴 경우

__스케줄러가 삭제 된 경우__

스케줄러 확인

```cmd
kubectl get pods -n kube-system
```
네임스페이스 `kube-system`에 스케줄러 `kube-scheduler-m-k8s`가 파드 형태로 존재

스케줄러 삭제

```cmd
kubectl delete pod kube-scheduler-m-k8s -n kube-system
```

다시 파드를 확인해 보면 스케줄러가 다시 생긴 것을 알 수 있음.  
마스터 노드에서의 중요 요소들은 특별 관리가 이루어진다

__Kubelet 중지 된 경우__

마스터 노드 Kubelet을 중지 시키고 

```cmd
systemctl stop kubelet
```
스케줄러를 삭제해 본다

스케줄러 삭제

```cmd
kubectl delete pod kube-scheduler-m-k8s -n kube-system
```
스케줄러가 terminating 상태가 되며

이 상태에서 디플로이먼트를 생성해보고

```cmd
kubectl create deployment nginx --image=nginx
```

생성되었는지 확인해 보면
```cmd
kubectl get pod
```
잘 생성된다

이 상태에서 스케일도 바꾸어 보면 

```cmd
kubectl scale deployment nginx --replicas=3
```

3개의 디플로이먼트가 잘 생성된다

curl로 확인 해 봐도

```cmd
curl <IP주소>
```
잘 작동이 된다

스케줄러가 terminating 상태여도 잘 동작이 되며 다시 Kubelet을 시작하면 다시 스케줄러가 run 상태로 바뀐다

__도커(컨테이너 런타임)이 중지 된 경우__

컨테이너 런타임을 중지하면

```cmd
systemctl stop docker
```

명령어 전달을 했을 때

```cmd
kubectl get pods
```

전달이 되지 않고

curl로 확인 해 봐도

```cmd
curl <IP주소>
```
동작하지 않는다

전체 시스템이 작동하지 않기 때문에 다시 컨테이너 런타임을 시작한다

```cmd
systemctl start docker
```

이러한 경우 때문에 실무에선 마스터 노드를 여러 개를 두고 사용하길 권장한다


<span style="font-size:70%">[참조] 인프런 - 쉽게 시작하는 쿠버네티스(v1.20)

끝!

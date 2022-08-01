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

__Kubelet 중지__

```cmd
systemctl stop kubelet
```
워커 노드의 kubelet을 중지시키고 배포하게 되면 제대로 배포되지 않고 pending 상태가 됨

```cmd
systemctl start kubelet
```
워커 노드의 kubelet을 다시 시작하고 배포하면 제대로 배포가 된다

__도커(컨테이너 런타임) 중지__

```cmd
systemctl stop docker
```
- 레플리카셋을 중지한 뒤에 배포를 하게 되면 중지된 워커 노드를 제외하고 배포
- 컨테이너 런타임에 문제가 생기면 Kubelet이 이를 인식해서 API 서버에 알림
- 스케줄러가 문제가 생긴 컨테이너에는 배포를 스케줄링 하지 않음
- 균형이 맞지 않는 상태에서 레플리카셋 값을 늘려주게 되면 스케줄러가 값에 따라 균형 맞게 컨테이너에 자동 배포


<span style="font-size:70%">[참조] 인프런 - 쉽게 시작하는 쿠버네티스(v1.20)

끝!

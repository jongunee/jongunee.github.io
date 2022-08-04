---
layout: post
title: 쿠버네티스 Tip
description: >
  인프런 - 쉽게 시작하는 쿠버네티스(v1.20) 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 쿠버네티스 Tip

* toc
{:toc .large-only}

## 간편 사용

__자동 완성__
```sh
yum install bash-completion -y
```
```sh
kubectl completion bash >/etc/bash_completion.d/kubectl
```

__단축어 사용__

```sh
echo 'alias k=kubectl' >> ~/.bashrc
```
```sh
echo 'complete -F __start_kubectl k' >> ~/.bashrc
```

kubectl 대신 k를 써도 동일한 명령 실행이 가능하다

__alias 사용__

```sh
alias k='kubectl'
alias kg='kubectl get'
alias kgp='kubectl get pods'
```

조건문을 걸어 더욱 편리하게 구성할 수도 있다

## 쿠버네티스 업그레이드

1. 업그레이드 계획 수립
2. kubeadm 업그레이드
3. kubelet 업그레이드
4. 업그레이드 완료 확인

__업그레이드 계획 수립__

먼저 현재 버전을 확인하고

```cmd
kubectl get nodes
```

최신 stable 버전들을 확인한다

```cmd
kubectl upgrade plan
```

__kubeadm 업그레이드__

현 kubeadm 버전과 설치 가능한 버전을 확인해서

```cmd
yum list kubeadm --showduplicates
```

kubeadm 최신 버전을 설치하고
```cmd
yum upgrade -y kubeadm-1.20.4
```

kubeadm 최신 버전을 설치한다
```cmd
kubeadm upgrade apply 1.20.4
```

버전 확인

```cmd
kubeadm version
```

__kubelet 업그레이드__

kubelet 버전이 그대로이면 쿠버네티스에 버전이 올라가지 않기 때문에 kubelet 버전을 업그레이드 한다

```cmd
yum upgrade kubelet-1.20.4 -y
```

버전 확인

```cmd
kubelet --version
```

재시작을 해줘야하기 때문에 재시작을 해주면

```cmd
systemctl restart kubelet
systemctl daemon-reload
```

__업그레이드 완료 확인__

버전이 업그레이드된 것 확인 가능

```cmd
kubectl get nodes
```

__워커 노드 버전 업그레이드__

kubeadm은 두고 kubelet만 업그레이드 후 재시작 해주면 버전 업그레이드가 된다

## 오브젝트 예약 단축어

모두 동일한 결과를 나타낸다

__파드 예약 단축어__

```cmd
pod
pods
po
```

__디플로이먼트 예약 단축어__

```cmd
deployment
deployments
deploy
```

__자주 사용되는 명령어__

| 이름 | 단축어 | 오브젝트 이름 |
| --- | --- | --- |
| nodes | `no` | Node |
| namespaces | `ns` | Namespace |
| deployments | `deploy` | Deployment |
| pods | `po` | Pod |
| services | `svc` | Service |


<span style="font-size:70%">[참조] 인프런 - 쉽게 시작하는 쿠버네티스(v1.20)

끝!

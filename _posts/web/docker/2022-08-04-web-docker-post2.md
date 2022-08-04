---
layout: post
title: 파드, ReplicaSet
description: >
  인프런 - 초보를 위한 쿠버네티스 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 파드, ReplicaSet

* toc
{:toc .large-only}

## 파드

**생성 과정**

1. `kubectl run`
2. `API 서버`가 `파드`를 생성하지만 아직 할당되지는 않음
3. `스케줄러`가 `API 서버`에 할당되지 않은 `파드`가 있는지 감시
4. 할당되지 않은 `파드`를 감지해 적절한 노드에 할당
5. `kubelet`은 자신의 노드에 할당된 `파드`가 있는지 체크
6. `kubelet`은 자신에 할당된 `파드` 정보를 확인해 `컨테이너` 생성
7. `kubelet`은 `API 서버`에 `파드` 상태를 전달

**설정 파일**

필수 요소 4가지

| 정의 | 설명 | 예시 |
| --- | --- | --- |
| version | 오브젝트 버전 | v1, app/v1, networking.k8s.io/v1 등 |
| kind | 리소스 종류 | Pod, ReplicaSet, Deployment, Service 등 |
| metadata | 메타데이터 | name, label, annotation으로 구성 |
| spec | 상세 명세 | 리소스 종류마다 다름 |

**파드 관련 명령어**

- 파드 생성
```cmd
kubectl apply -f <파일명>.yml
```

- 파드 생성
```cmd
kubectl get pod
```

- 파드 로그 확인
```cmd
kubectl logs echo
```

- 파드 컨테이너 접속
```cmd
kubectl exec -it echo -- sh
```

- 파드 제거
```cmd
kubectl delete -f <파일명>.yml
```

- 파드 생성
```cmd
kubectl get pod
```

**livenessProbe**

- 컨테이너가 정상적으로 동작하는지를 체크
- 정상적으로 동작하지 않는다면 컨테이너를 재시작해서 문제 해결
- `httpGet`, `tcpSocket`, `exec` 방법 등으로 체크

**readinessProbe**

- 컨테이너가 준비되었는지를 체크
- 정상적으로 준비되지 않았다면 파드로 들어오는 요청 제외
- 재시작하는 것이 아니라 요청만 제외한다는 것이 차이점
- 실제 서비스에서는 `LivenessProbe`와 `readinessProbe` 모두 적용

## ReplicaSet

**ReplicaSet이란?**

>  파드를 단독으로 만들면 파드가 죽는 등의 문제가 생겼을 때 자동으로 복구되지 않는다. 이러한 파드를 정해진 수만큼 복제하고 관리하는 것이 <span style='background-color: #f5f0ff'>ReplicaSet</span>

**설정 yml 파일**

```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: echo-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: echo
      tier: app
  template:
    metadata:
      labels:
        app: echo
        tier: app
    spec:
      containers:
        - name: echo
          image: ghcr.io/subicura/echo:v1
```

- replicas를 3으로 설정했기 때문에 3개의 컨테이너를 띄우도록 유지
- matchLabels 항목들이 동일한 컨테이너를 확인
- 없다면 template 설정대로 컨테이너 생성

**ReplicaSet 동작 과정**

1. `ReplicaSet Controller`가 `API 서버`에 `ReplicaSet` 조건을 계속해서 체크
2. 조건이 맞지 않다면 만족시키기 위해 파드 생성/제거
3. `스케줄러`가 `API 서버`에 할당되지 않은 `파드`가 있는지 감시
4. `스케줄러`가 파드를 노드에 할당


<span style="font-size:70%">[참조] 인프런 - 초보를 위한 쿠버네티스 안내서

끝!

---
layout: post
title: Helm Hook Flow
description: >
  [참조]: 인프런 - 대세는 쿠버네티스 [Helm편]
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# Helm Hook Flow

* toc
{:toc .large-only}

## Chart Template 생성

```cmd
helm create mychart
```

불필요한 파일 삭제

```cmd
rm -rf deployment.yaml  hpa.yaml  ingress.yaml  service.yaml  serviceaccount.yaml  tests/test-connection.yaml
```

## Pod 및 Deployment 생성

**pre-pod.yaml 생성**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: pre-pod
  annotations:
    helm.sh/hook: pre-upgrade
spec:
  restartPolicy: Never
  containers:
  - name: container
    image: kubetm/init
    command: ["sh", "-c", "echo 'start'; sleep 10; echo 'done'"]
```

- 배포 전에 확인차 만들어지는 Pre-pod

**deployment.yaml 생성**

```yml
apiVersion: v1
kind: Service
metadata:
  name: svc
spec:
  selector:
    type: app
  ports:
  - port: 80
    targetPort: 8080
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment
spec:
  selector:
    matchLabels:
      type: app
  replicas: 1
  template:
    metadata:
      labels:
        type: app
      annotations:
        rollme: {{ randAlphaNum 5 | quote }}
    spec:
      initContainers:
      - name: init-myservice
        image: kubetm/app
        command: ["sh", "-c", "echo 'start'; sleep 10; echo 'done'"]
      containers:
      - name: container
        image: kubetm/app
```

**post-pod.yaml 생성**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: post-pod
  annotations:
    helm.sh/hook: post-upgrade
spec:
  restartPolicy: Never
  containers:
  - name: container
    image: kubetm/init
    command: ["sh", "-c", "echo 'start'; sleep 10; echo 'done'"]
```

**tests/test-pod.yaml 생성**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
  annotations:
    helm.sh/hook: test
spec:
  restartPolicy: Never
  containers:
  - name: container
    image: kubetm/init
    command: ["sh", "-c", "echo 'start'; curl svc/hostname; echo 'done'"]
```

**crds/crd-pod.yaml**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: crd-pod
  annotations:
    helm.sh/hook: pre-upgrade
spec:
  restartPolicy: Never
  containers:
  - name: container
    image: kubetm/init
    command: ["sh", "-c", "echo 'start'; sleep 10; echo 'done'"]
```

## Install

```cmd
helm upgrade mychart ./../ -n nm-1 --create-namespace --install
kubectl get pods -n nm-1
```

## Upgrade

```cmd
helm upgrade mychart ./../ -n nm-1 --create-namespace --install
kubectl get pods -n nm-1
```

## Test

```cmd
helm test mychart -n nm-1
kubectl get pods -n nm-1
```

- 앱을 배포하기 전에 확인을 해보고 성공시에만 배포를 하는 경우 등 App의 기능을 보조
- 쿠버네티스 리소스 생성시 우선순위 할당 가능



<span style="font-size:70%">[참조]: 인프런 - 대세는 쿠버네티스 [Helm편]

끝!

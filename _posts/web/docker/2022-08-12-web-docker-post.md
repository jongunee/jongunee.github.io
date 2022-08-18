---
layout: post
title: 클러스터 구성
description: >
  Microk8s 사용 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 클러스터 구성

* toc
{:toc .large-only}

## 배포


<span style="font-size:70%">[참조]: [Canocical Microk8s 공식 문서](https://microk8s.io/docs/)

끝!

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
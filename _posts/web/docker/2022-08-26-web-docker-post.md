---
layout: post
title: Kustomize - Metadata, Replicas, Generators
description: >
  [참조]: 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# Kustomize - metadata

* toc
{:toc .large-only}

## Metadata 커스터마이징

**yaml 파일**

```yml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- deployment.yaml
- service.yaml

namespace: jw_ns
namePrefix: dev-
nameSuffix: -devops

commonLabels:
  department: "keti"
  owner: "jw"
commonAnnotations:
  key1: "val1"
  key2: "val2"
```

**결과 metadata**

```yml
metadata:
  annotations:
    key1: val1
    key2: val2
  labels:
    department: keti
    owner: jw
  name: dev-hello-devops
  namespace: jw_ns
```

- 기존 `yaml` 설정 파일을 수정하지 않고 `kustomize` 파일만 수정
- metadata에 설정 값 추가 가능

## Replicas & Images

**yaml 파일**

```yml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- grafana/
- hello/

replicas:
- name: grafana
  count: 2
- name: hello
  count: 1

images:
- name: grafana/grafana
  newTag: "8.2.2"
- name: nginxdemos/hello
  newName: nginx
  newTag: "latest"
```

- resources:
  - `resources` 설정으로 디렉토리로 넣어줄 때에는 해당 디렉토리에 `kustomization.yaml` 파일이 반드시 존재해야 함
- replicas:
  - `grafana/kustomization.yaml` 파일이나 `hello/kustomization.yaml` 파일에서 `replicas` 설정이 되어 있더라도 최상단 디렉토리에서 설정한 `replicas`로 덮어쓰게 됨
- images:
  - 레지스트리 위치를 변경 가능
  - 이미지 버전 변경이 가능

## Generators

**yaml 파일**

```yml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- deployment.yaml
- service.yaml

configMapGenerator:
- name: mysql-config
  literals:
  - MYSQL_DATABASE=devops
  envs:
  - mysql.env
- name: test-files
  files:
  - files/test1.txt
  - test2.txt=files/test2.txt

secretGenerator:
- name: mysql-secret
  literals:
  - MYSQL_ROOT_PASSWORD=1234

generatorOptions:
  labels:
    env: dev
  annotations:
    managed_by: kustomize
  # disableNameSuffixHash: true
```

- configMapGenerator
  - 쿠버네티스 `configMap` 설정 및 생성
  - `literals`, `envs`, `files` 속성 설정 가능
- secretGenerator
  - `ConfigMapGenerator`와 설정 동일
- generatorOptions
  - `configMapGenerator`, `secretGenerator`에 일반적인 옵션 제공
  - `labels`, `annotations`는 모든 `configMap`과 `secret`에 적용됨
  - `disableNameSuffixHash`를 `true`로 설정해 주면 hash 코드로 변환하지 않음 


<span style="font-size:70%">[참조]: 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online

끝!

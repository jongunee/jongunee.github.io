---
layout: post
title: ConfigMap, Secret
description: >
  인프런 - 초보를 위한 쿠버네티스 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# ConfigMap, Secret

* toc
{:toc .large-only}

## ConfigMap

> 컨피그맵은 Key-Value 쌍으로 기밀이 아닌 데이터를 저장하는 데 사용하는 API 오브젝트

컨테이너에서 필요한 환경설정 내용을 컨테이너와 분리해서 제공해 주기 위한 기능이다.

**ConfigMap 생성**

환경파일 소스 코드

```yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: prometheus
    metrics_path: /prometheus/metrics
    static_configs:
      - targets:
          - localhost:9090
```

파일을 통째로 ConfigMap으로 만들어 컨테이너에서 사용할 것

ConfigMap 생성

```cmd
kubectl create cm my-config --from-file=config-file.yml
```
ConfigMap 조회
```cmd
kubectl get cm
```
ConfigMap 내용 상세 조회
```cmd
kubectl describe cm/my-config
```

**ConfigMap 연결**

소스 코드

```yml
apiVersion: v1
kind: Pod
metadata:
  name: alpine
spec:
  containers:
    - name: alpine
      image: alpine
      command: ["sleep"]
      args: ["100000"]
      volumeMounts:
        - name: config-vol
          mountPath: /etc/config
  volumes:
    - name: config-vol
      configMap:
        name: my-config
```

- `alpine` 컨테이너에서 공유할 경로를 `config-vol` 볼륨으로 지정
- `config-vol` 볼륨을 위에서 생성한 `my-config` ConfigMap과 연결

## env파일로 연결

env 포멧

```yml
hello=world
haha=hoho
```

ConfigMap 생성

```cmd
kubectl create cm env-config --from-env-file=config-env.yml
```

ConfigMap 조회

```cmd
kubectl describe cm/env-config
```

## yml 파일 연결

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  hello: world
  kuber: netes
  multiline: |-
    first
    second
    third
```

configmap 생성

```cmd
kubectl apply -f config-map.yml
```

alpine 적용

```cmd
kubectl apply -f alpine.yml
```

적용내용 확인

```cmd
kubectl exec -it alpine -- cat /etc/config/multiline
```

## ConfigMap 환경변수로 사용

```yml
apiVersion: v1
kind: Pod
metadata:
  name: alpine-env
spec:
  containers:
    - name: alpine
      image: alpine
      command: ["sleep"]
      args: ["100000"]
      env:
        - name: hello
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: hello
```

- 앞서 만든 `my-config` ConfigMap에서 환경 변수를 불러옴
- `key`-`value로` `hello`: `world`가 담김

컨테이너 생성

```cmd
kubectl apply -f alpine-env.yml
```

env 확인

```cmd
kubectl exec -it alpine-env -- env
```

## Secret

> Secret은 쿠버네티스에서 비밀번호, OAuth 토큰, ssh 키 등의 민감한 보안 정보를 관리하는 데 사용된다.

- Secret은 보안 정보를 다루지만 암호화되지 않고 실제 그대로 저장됨
- etcd에 그대로 저장이 되기 때문에 유출 가능성이 있음
- 별도 암호화 모듈을 사용해 보안 강화 가능

**Secret 생성**

username.txt

```
admin
```

password.txt

```
qwer1234
```

Secret 생성

```cmd
kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
```

secret 상세 조회

```cmd
kubectl describe secret/db-user-pass
```

-o yaml로 상세 조회

```cmd
kubectl get secret/db-user-pass -o yaml
```

저장된 데이터 base64 decode

```cmd
echo 'MXEydzNlNHI=' | base64 --decode
```

디코딩되어 그대로 출력이 된다.

**Secret 환경변수 연결**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: alpine-env
spec:
  containers:
    - name: alpine
      image: alpine
      command: ["sleep"]
      args: ["100000"]
      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: db-user-pass
              key: username.txt
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-user-pass
              key: password.txt
```

- `env` 환경변수로 `secret`의 `username`, `password`를 불러온다

컨테이너 생성

```cmd
kubectl apply -f alpine-env.yml
```

env 확인

```cmd
kubectl exec -it alpine-env -- env
```

<span style="font-size:70%">[참조] 인프런 - 초보를 위한 쿠버네티스 안내서

끝!

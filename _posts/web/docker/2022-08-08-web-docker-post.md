---
layout: post
title: Ingress, Volume
description: >
  인프런 - 초보를 위한 쿠버네티스 안내서 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# Ingress, Volume

* toc
{:toc .large-only}

## Ingress

>Ingress란? 각 서비스를 생성하고 접근할 때 고유 포트를 입력해주어야 하는데 서비스가 늘어날수록 관리가 어려워진다. <span style='background-color: #f5f0ff'>Ingress</span>는 서비스 포트를 적지 않고 경로로 접근이 가능하도록 해준다

**microk8s Ingress 활성화**

```cmd
microk8s enable ingress
```

**Ingress 컨트롤러 확인**

```cmd
kubectl get pods -A | grep ingress
```

**Ingress 동작 순서**

1. `Ingress Controoler`가 `API 서버`에서 `Ingress` 변화 감시
2. `Ingress Controoler`가 설정 파일을 보고 `Nginx`의 설정 변경
3. 프로세스 재시작

**예제 코드**

```yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx
spec:
  rules:
    - host: nginx.192.168.64.5.sslip.io
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx
                port:
                  number: 80

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: echo
          image: nginx:latest

---
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  ports:
    - port: 80
      protocol: TCP
  selector:
    app: nginx
```

- sslip를 사용해서 동일 IP, 동일 포트라도 다른 파드를 실행 시킬 수 있음
- `nginx.192.168.64.5.sslip.io`: 해당 주소로 

## Volume

> 볼륨은 파드의 일부분으로 정의되며 파드와 동일한 라이프사이클을 갖는 디스크 스토리지이다. 

만들었던 컨테이너들은 파드를 제거하면 내부에 저장된 데이터가 모두 사라지게 된다. 예를들어 워드프레스 컨테이너에서 데이터베이스에 따로 저장을 하지 않는다면 컨테이너 삭제시 데이터가 모두 사라진다.

<span style='background-color: #f5f0ff'>볼륨</span>을 이용하면 컨테이너 디렉토리를 외부 저장소와 연결해 문제점을 해결할 수 있다

**empty-dir**

> 파드 안의 컨테이너 간 동일 디렉토리를 공유할 때 사용

**예제 코드**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: sidecar
spec:
  containers:
    - name: app
      image: busybox
      args:
        - /bin/sh
        - -c
        - >
          while true;
          do
            echo "$(date)\n" >> /var/log/example.log;
            sleep 1;
          done
      volumeMounts:
        - name: varlog
          mountPath: /var/log
    - name: sidecar
      image: busybox
      args: [/bin/sh, -c, "tail -f /var/log/example.log"]
      volumeMounts:
        - name: varlog
          mountPath: /var/log
  volumes:
    - name: varlog
      emptyDir: {}
```

- `app`과 `sidebar`에서 경로를 `varlog` 이름으로 공유 
- `app`이 `varlog` 경로에 있는 `example.log` 파일에 1초마다 시간 로그 생성
- `sidebar`가 `varlog` 경로에 있는 `example.log` 파일의 로그를 읽음

**hostpath**

> 컨테이너 디렉토리를 외부(호스트) 디렉토리와 연결을 할 떄 사용

```yml
apiVersion: v1
kind: Pod
metadata:
  name: host-log
spec:
  containers:
    - name: log
      image: busybox
      args: ["/bin/sh", "-c", "sleep infinity"]
      volumeMounts:
        - name: varlog
          mountPath: /host/var/log
  volumes:
    - name: varlog
      hostPath:
        path: /var/log
```
- `log` 컨테이너는 `sleep inifinity`로 연결을 유지하는 역할만 함
- `log` 컨테이너와 외부 호스트는 경로를 `varlog` 이름으로 공유
- `log` 컨테이너에서 호스트에 저장되어 있던 로그 확인 가능


<span style="font-size:70%">[참조] 인프런 - 초보를 위한 쿠버네티스 안내서

끝!

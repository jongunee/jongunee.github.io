---
layout: post
title: Helm Chart 생성
description: >
  [참조]: 인프런 - 대세는 쿠버네티스 [Helm편]
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# Helm Chart 생성

* toc
{:toc .large-only}

## Helm Chart 기본 구조

create 명령어를 통해 생성 된 기본 구조 

```cmd
jw@jw-pc:~/k8s/helm/mychart$ tree
.
├── Chart.yaml  
├── charts  
├── templates  
│   ├── NOTES.txt  
│   ├── _helpers.tpl  
│   ├── deployment.yaml  
│   ├── hpa.yaml  
│   ├── ingress.yaml  
│   ├── service.yaml  
│   ├── serviceaccount.yaml  
│   └── tests  
│       └── test-connection.yaml  
└── values.yaml  
```

| 파일 및 폴더명 | 설명 |
| --- | --- |
| Chart.yaml | 차트에 대한 이름, 버전, 설명 등 차트 정보 파일 |
| charts/ | 차트와 관련 있는 차트들을 포함한 디렉토리 |
| templates/ | 쿠버네티스 manifest 파일들로 변환될 템플릿을 포함한 디렉토리 |
| _helpers.ypl | template manifest 파일들에서 공유하는 변수 정의 |
| values.yaml | 차트 설치시 사용할 기본 설정 정의 |

## Helm Chart 생성

**Chart Template 생성**

```cmd
helm create mychart
```

**Show 명령어**

```
helm show values .
helm show chart .
helm show readme .
helm show all .
```
yaml 파일에서 주석을 제외한 코드를 보여준다

README 파일 생성

```cmd
vim README.md
```

**Template 명령어**

```cmd
helm template mychart .                  
```

만들어진 헬름 차트를 미리 확인 가능

**Chart 배포**

```cmd
helm install mychart .
```

**Get 명령어**

```cmd
helm get manifest mychart
helm get notes mychart
helm get values mychart
helm get all mychart
```

배포된 인스턴스에 대한 정보를 출력

```cmd
helm get values
```

그냥 차트만 배포했을 경우 아무런 출력을 하지 않는다

```cmd
helm install mychart . -f values.yaml
helm install mychart . --set replicaCount=3
```

`helm get values` 명령어는 배포시에 옵션으로 준 input 파라미터에 대한 내용을 조회하는 명령어이기 때문에 input 파라미터를 넣어주어야만 출력을 확인할 수 있다

## 내장 객체

**ConfigMap 추가**

```cmd
vim templates/cm-object.yaml
```

```cmd
apiVersion: v1
kind: ConfigMap
metadata:
  name: built-in-object
data:
  .Release: ______________________________________
  .Release.Name: {{ .Release.Name }}
  .Release.Namespace: {{ .Release.Namespace }}
  .Release.IsUpgrade: "{{ .Release.IsUpgrade }}"
  .Release.IsInstall: "{{ .Release.IsInstall }}"
  .Release.Revision: "{{ .Release.Revision }}"
  .Release.Service: {{ .Release.Service }}
  .Values: ______________________________________
  .Values.replicaCount: "{{ .Values.replicaCount }}"
  .Values.image.repository: {{ .Values.image.repository }}
  .Values.image.pullPolicy: {{ .Values.image.pullPolicy }}
  .Values.service.type: {{ .Values.service.type }}
  .Values.service.port: "{{ .Values.service.port }}"
  .Chart: ______________________________________
  .Chart.Name: {{ .Chart.Name }}
  .Chart.Description: {{ .Chart.Description }}
  .Chart.Type: {{ .Chart.Type }}
  .Chart.Version: {{ .Chart.Version }}
  .Chart.AppVersion: {{ .Chart.AppVersion }}
  .Template: ______________________________________
  .Template.BasePath: {{ .Template.BasePath }}
  .Template.Name: {{ .Template.Name }}
```

**install 배포**

```cmd
helm install mychart . -n default
helm get manifest mychart
```

확인해보면

```yaml
.Release.IsUpgrade: "false"
.Release.IsInstall: "true"
.Release.Revision: "1"
.Release.Service: Helm
.Values: ______________________________________
.Values.replicaCount: "1"
```

`values.yaml` 파일 변경

```yaml
replicaCount:3
```

차트 업그레이드

```cmd
helm upgrade mychart . -n default
```

확인해보면

```yaml
.Release.IsUpgrade: "true"
.Release.IsInstall: "false"
.Release.Revision: "2"
.Release.Service: Helm
.Values: ______________________________________
.Values.replicaCount: "2"
```

바뀐 것 확인 가능


## 변수 주입 우선순위

**ConfigMap 추가**

```cmd
vim templates/cm-vaules.yaml
```

**`values.yaml` 파일에 속성 추가**

```yaml
configMapData:
  env: dev
  log: debug
  path: "/data"
```

**결과 확인**

```cmd
helm template mychart .
```
```yaml
data:
  env: dev
  log: debug
  path: "/data"
```

---

**Prod용 `values_prod.yaml` 파일 추가**

```cmd
vim values_prod.yaml
```

```yaml
configMapData:
  env: prod
  log: info
```

**-f 옵션 적용 및 결과 확인**

```cmd
helm template mychart . -f ./values_prod.yaml
```

```yaml
data:
  env: prod
  log: info
  path: "/data"
```

---

**-f 옵션 적용, -set 옵션 적용 및 결과 확인**

```cmd
helm template mychart . -f ./values_prod.yaml --set configMapData.log=debug
```

```yaml
data:
  env: prod
  log: debug
  path: "/data"
```

<mark>우선순위</mark>: set > -f > values.yaml






<span style="font-size:70%">[참조]: 인프런 - 대세는 쿠버네티스 [Helm편]

끝!

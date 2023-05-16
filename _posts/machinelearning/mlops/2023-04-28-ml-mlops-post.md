---
layout: post
title: Kubeflow란? + minikube 클러스터 Kubeflow 설치
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# Kubeflow란? + minikube 클러스터 Kubeflow 설치

* toc
{:toc .large-only}

## Kubeflow 워크플로우

![그림1](https://www.kubeflow.org/docs/images/kubeflow-overview-workflow-diagram-1.svg)

<span style="font-size:70%">출처: https://www.kubeflow.org/docs/started/architecture/</span>

Kubeflow는 MSA 구조로 각각 모듈 형태로 동작

- Experimental phase
  - 문제 식별, 데이터 수집
  - ML 알고리즘 선택, 코드화  
    ex) PyTorch, scikit-learn, TensorFlow, XGBoost
  - 데이터 모델 학습  
    ex) Jupyter Notebook, Fairing, Pipelines
  - 하이퍼파라미터 튜닝  
    ex) Katib

- Production phase
  - 데이터 전처리
  - 모델 학습  
    ex) Chainer, MPI, MXNet, PyTorch, TFJob
  - 모델 서빙  
    ex) KFServing, NVIDIA TensorRT, PyTorch, TFServing, Seldon
  - 모델 퍼모먼스 모니터링  
    ex) Metadata, TensorBoard

**Kubeflow Pipelines**

- 머신러닝 workflow를 DAG(방향 순환 없는 그래프) 형태로 정의한 것
- kubeflow에서 파이프라인을 run 하면 각 컴포넌트들이 파드로 생성되어 서로 데이터를 주고 받으며 흘러감
- 모델을 서빙 단계까지 보내는데 필요한 모든 작업을 컴포넌트 단위로 나누고 쿠버네티스 위에서 연결시키는 역할
- 목표
  - **End to End Orchestration:** 모델 연구 및 학습 과정과 서빙 과정의 괴리를 없애는 것
  - **Easy Experimentation:** 쉽게 다양한 설정에 따른 실험
  - **Easy Re-Use:** 파이프라인의 컴포넌트들을 새로운 파이프라인에 재사용하여 작업 효율 향상
- 파이썬 SDK를 이용해 파이썬 코드를 작성하고 컴파일러를 통해 yaml 파일로 변환하여 파이프라인 생성

## Kubeflow 설치

### 환경 요건

- 쿠버네티스
- Dynamic provisioning을 지원하는 storageclass
- TokenRequest API 활성화

**Kustomize 설치**

```cmd
wget https://github.com/kubernetes-sigs/kustomize/releases/download/v3.2.0/kustomize_3.2.0_linux_amd64
```

```cmd
chmod +x kustomize_3.2.0_linux_amd64
```

실행 가능하도록 파일 형식을 변경한다.

```cmd
sudo mv kustomize_3.2.0_linux_amd64 /usr/local/bin/kustomize
```
kustomize 명령어를 이용해 실행할 수 있도록 파일 위치를 변경한다.

**kubens 설치**

```cmd
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

kubens 명령어를 이용해서 네임스페이스를 지정할 수 있음

**minikube 시작**

```cmd
minikube start --driver=docker \
  --cpus='4' --memory='7g' \
  --kubernetes-version=v1.21.0 \
  --extra-config=apiserver.service-account-signing-key-file=/var/lib/minikube/certs/sa.key \
  --extra-config=apiserver.service-account-issuer=kubernetes.default.svc
```

**manifest 설정**

```cmd
git clone https://github.com/kubeflow/manifests.git
```

### Kubeflow 컴포넌트 설치

**Cert-manager 설치**

```cmd
kustomize build common/cert-manager/cert-manager/base | kubectl apply -f -
kustomize build common/cert-manager/kubeflow-issuer/base | kubectl apply -f -
```

**Istio 설치**

```cmd
kustomize build common/istio-1-9/istio-crds/base | kubectl apply -f -
kustomize build common/istio-1-9/istio-namespace/base | kubectl apply -f -
kustomize build common/istio-1-9/istio-install/base | kubectl apply -f -
```

**Dex 설치**

```cmd
kustomize build common/dex/overlays/istio | kubectl apply -f -
```

**Kubeflow Namespace 생성**

```cmd
kustomize build common/kubeflow-namespace/base | kubectl apply -f -
```

**Kubeflow Roles 생성**

```cmd
kustomize build common/kubeflow-roles/base | kubectl apply -f -
```

**Kubeflow Gateway 설치**

```cmd
kustomize build common/istio-1-9/kubeflow-istio-resources/base | kubectl apply -f -
```

**Kubeflow Pipelines 설치**

```cmd
kustomize build apps/pipeline/upstream/env/platform-agnostic-multi-user | kubectl apply -f -
```

오류가 나면 한 번 더 명령어를 입력해서 설치 가능

**Katib 설치**

```cmd
kustomize build apps/katib/upstream/installs/katib-with-kubeflow | kubectl apply -f -
```

**Central Dashboard 설치**

```cmd
kustomize build apps/centraldashboard/upstream/overlays/istio | kubectl apply -f -
```
여기까지
**Notebooks 설치**

```cmd
kustomize build apps/jupyter/notebook-controller/upstream/overlays/kubeflow | kubectl apply -f -
```

Notebook Controller가 설치된다.

```cmd
kustomize build apps/jupyter/jupyter-web-app/upstream/overlays/istio | kubectl apply -f -
```

Jupyter Web App이 설치된다.

**Profiles + KFAM 설치**

```cmd
kustomize build apps/profiles/upstream/overlays/kubeflow | kubectl apply -f -
```

**Volumes Web App 설치**

```cmd
kustomize build apps/volumes-web-app/upstream/overlays/istio | kubectl apply -f -
```

**Tensorboard 설치**

```cmd
kustomize build apps/tensorboard/tensorboards-web-app/upstream/overlays/istio | kubectl apply -f -
```

Tensorboard Web App이 설치된다.

```cmd
kustomize build apps/tensorboard/tensorboard-controller/upstream/overlays/kubeflow | kubectl apply -f -
```

Tensorboard Controller가 설치된다.

**User Namespace 설치**

```cmd
kustomize build common/user-namespace/base | kubectl apply -f -
```

**kubeflow 접속**

```cmd
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
```
`localhost:8080`으로 접속






<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps</span>

끝!

---
layout: post
title: EKS 환경 kubeflow 구축
description: >
  [참조] https://github.com/aws-samples/amazon-efs-developer-zone/tree/main/application-integration/container/eks/kubeflow
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# EKS 환경 kubeflow 구축

* toc
{:toc .large-only}

## Kubeflow 1.7 호환성 확인

**Kubeflow 1.7 컴포넌트 버전**

![그림1](/assets/img/ml/kubeflow_component_versions.jpg)


**Kubeflow 1.7 Dependency 버전** 

![그림2](/assets/img/ml/kubeflow_dependency_versions.jpg)

## 클러스터 세팅

**필요 툴 확인**

```cmd
kubectl version
aws --version
jq --version
yq --version
```

**클러스터 확인**

```cmd
kubectl get nodes
```

## Kubeflow 설치

**Kustomize 설치**

```cmd
wget -O kustomize.tar.gz https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv5.0.0/kustomize_v5.0.0_linux_amd64.tar.gz
```

**Kustomize 압축 풀기**

```cmd
gzip -d kustomize.tar.gz 
tar xvf kustomize.tar 
```

```cmd
chmod +x kustomize
sudo mv -v kustomize /usr/local/bin
```

**Kustomize 실행 확인**

```cmd
kustomize version
```

**Kubeflow 설치**

```cmd
git clone https://github.com/kubeflow/manifests.git
cd manifast
```

```cmd
while ! kustomize build deployments/vanilla | kubectl apply --validate=false -f -; do echo "Retrying to apply resources"; sleep 30; done
```

```cmd
kubectl get pods -n cert-manager
kubectl get pods -n istio-system
kubectl get pods -n auth
kubectl get pods -n knative-eventing
kubectl get pods -n knative-serving
kubectl get pods -n kubeflow
kubectl get pods -n kubeflow-user-example-com
```



<span style="font-size:70%">https://github.com/aws-samples/amazon-efs-developer-zone/tree/main/application-integration/container/eks/kubeflow

끝!

---
layout: post
title: kubeflow Katib
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# Kubeflow Katib

* toc
{:toc .large-only}

## Kubeflow Katib이란

> Katib는 AutoML 프로젝트를 위한 프로젝트로 하이퍼파라미터 튜닝, Early Stopping, Neural Architecture Search를 제공

**Katib의 특징**

- Cluster Management  
Kubernetes-native로 설계되어 클러스터 관리 시스템을 추가 구축, 관리할 필요가 없음

- Agnostic to ML Framework  
HPO를 수행할 모델이 어떤 ML 프레임워크로 개발이 되었든 해당 학습 모델을 컨테이너화만 시킬 수 있다면 동작 가능

- Support Scikit-Optimize, HyperOpt, Optuna 등:  
Katib의 인터페이스로 HPO(HyperParameter Optimization) 위한 외부 라이브러리 사용 가능

**Katib 컨셉**

![그림1](/assets/img/ml/katib_concept.png)

- 여러 쿠버네티스 CR(Custom Resource)을 통해 동작
- 하나의 Expoertiment가 여러 Suggestion에 대응됨
- Suggestion, Trial, Worker Job, Pod 모두 1대1 대응
- Experiment:  
HPO 전체 과정 1회(Objective + Search space + Search algo), Kubeflow 탭에서는 Experiments(AutoML)을 말함
- Suggestion:  
후보 HP 조합 1세트

## Katib 예제 1

**예제 코드**

```yaml
apiVersion: "kubeflow.org/v1beta1"
kind: Experiment
metadata:
  namespace: kubeflow-user-example-com
  name: demo

spec:
  objective:
    type: maximize
    goal: 0.99
    objectiveMetricName: Validation-accuracy
    additionalMetricNames:
      - Train-accuracy

  algorithm:
    algorithmName: random

  parallelTrialCount: 2
  maxTrialCount: 2
  maxFailedTrialCount: 2 
  parameters:
    - name: lr 
      parameterType: double
      feasibleSpace:
        min: "0.01"
        max: "0.03"
    - name: num-layers
      parameterType: int
      feasibleSpace:
        min: "2"
        max: "5"
    - name: optimizer
      parameterType: categorical
      feasibleSpace:
        list:
          - sgd
          - adam
          - ftrl

  trialTemplate:
    primaryContainerName: training-container
    trialParameters:
      - name: learningRate
        description: Learning rate for the training model
        reference: lr
      - name: numberLayers
        description: Number of training model layers
        reference: num-layers
      - name: optimizer
        description: Training model optimizer (sdg, adam or ftrl)
        reference: optimizer

    trialSpec:
      apiVersion: batch/v1
      kind: Job
      spec:
        template:
          metadata:
            annotations:
              sidecar.istio.io/inject: 'false'
          spec:
            containers:
              - name: training-container
                image: docker.io/kubeflowkatib/mxnet-mnist:v1beta1-45c5727 
                command:
                  - "python3"
                  - "/opt/mxnet-mnist/mnist.py"
                  - "--batch-size=64"
                  - "--lr=${trialParameters.learningRate}"
                  - "--num-layers=${trialParameters.numberLayers}"
                  - "--optimizer=${trialParameters.optimizer}"
                  - "--num-epochs=1"
            restartPolicy: Never
```

- `Spec`: Experiment 관련 메타 정보 작성
- `Objective`: 최적화하기 위한 metric, type, early stopping goal 등 포함
- `algorithm`: Search 알고리즘에 대한 정보이며 다음 명령어로 suggestion 필드 확인
  ```cmd
  kubectl get configmap katib-config -o yaml
  ```
- `Parameters`: 각 하이퍼파라미터마다 타입, space는 무엇인지 등 정의
- `trialSpec`: Job, TfJob 등 리소스 사용 가능

**예제 1 실행**

```cmd
kubens kubeflow-user-example-com
```
네임스페이스를 변경해준다.

```cmd
kubectl apply -f random-example.yaml
```
위 yaml 파일을 `kubectl`로 생성해준다.

```cmd
watch -n1 'kubectl get experiment,suggestion,trial,pod'
kubectl logs <pod-name> -c metrics-logger-and-collector
```

Kubeflow Expertiments(AutoML) UI에서도 생성이 가능하다.

**예제2**

```yaml
  algorithm:
    algorithmName: bayesianoptimization

    algorithmSettings:
      - name: "random_state"
        value: "1234"
      - name: "n_initial_points"
        value: "5"
```
- 위 예제 코드에서 `algorithm` 부분만 바꿔서 실행

**예제3**

```yaml
  metricsCollectorSpec:
    collector:
      kind: File

    source:
      fileSystemPath:
        path: "/katib/mnist.log"
        kind: File
      filter:
        metricsFormat:
          - "{metricName: ([\\w|-]+), metricValue: ((-?\\d+)(\\.\\d+)?)}"
```
- 해당 부분을 추가해서 실행
- 기존에는 `metricsCollectorSpec`에서 Default인 `StdOut`이 사용되었으나 `File`로 사용



<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps</span>

끝!

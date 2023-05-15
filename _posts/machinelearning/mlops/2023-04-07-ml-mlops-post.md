---
layout: post
title: MLflow
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# MLflow

* toc
{:toc .large-only}

## Model Management

**ML Model의 라이프사이클**

Raw Data -> Data Processing -> Train & Evaluate의 지속적인 반복

최종적인 모델을 기록해 두기 위해서는 다음 정보들이 필요

- 모델 소스코드
- Evaluation Metri 결과
- 사용한 파라미터
- Model.pkl 파일
- 학습에 사용한 데이터
- 데이터 전처리용 코드
- 전처리된 데이터 등

이와 같은 정보들이 있어야 해당 모델을 비교, 재현이 가능해진다.

**Model Management Tools**

- MLflow
- Tensorboard
- Neptune
- Weights & Biases
- Comet.ml

## MLflow

- MLflow tracking
  - 모델의 하이퍼파라미터나 코드 등을 바꿔가며 실험할 때 각 모델 버전의 메트릭을 저장하는 중앙 저장소
  - 메타 정보를 모델과 함께 기록할 수 있는 API 제공
  - 성능, 파라미터, 소스코드 버전, 날짜, 패키징된 모델 자체, 데이터 자체, 주석 등 모두 저장
- MLflow projects
  - 모델 학습코드가 성능을 재현할 수 있도록 파라미터의 데이터 타입, 툴 버전 등 모델의 의존성이 있는 모든 정보드를 넣어서 코드 패키징
- MLflow models
  - 항상 통일된 형태로 배포할 수 있도록 포맷화
  - 어떤 언어로 개발했던지 특정 프레임워크나 언어에 종속되지 않고 동일한 형태로 배포될 수 있도록 함
- MLflow model registry
  - MLflow로 실험했던 모델을 저장하고 관리하는 저장소
  - 실험한 모델 서빙

**MLflow Tracking 구조**

- 서버-클라이언트 구조
- Tacking 서버:
  - 로컬 파일 시스템, DB를 백엔드에서 사용해 클라이언트가 저장해달라고 요청한 모델 또는 관련 메타정보들을 Storage에 저장
  - 클라이언트 요청에 따라 DB 정보 반환
- 모델과 관련된 큰 데이터는 S3 등 저장소에 저장, 연동도 가능
- UI 제공

**MLflow 장점**

- 쉬운 설치
- 쉬운 Migration
- 대시보드 제공
- 다양한 Client API 제공
- 다양한 Backend Storage 연동 지원
- 다양한 Artifact Storage 연동 지원 등

## MLflow 실행

**MLflow 설치**

```cmd
pip install mlflow
```

**MLflow Tracking Server 열기**

```cmd
mlflow ui
```

또는 

```cmd
mlflow server
```

- 둘 다 비슷하지만 Production 용으로는 MLflow Server로 권장됨
- MLflow Server는 로컬 뿐만아니라 S3 스토리지와 연동 가능

```cmd
cat mlruns/meta.yaml
```
종료 후 기본 경로인 mlruns 경로에 관련 데이터들이 채워진 것을 확인 할 수 있다.

## MLflow 확인

```cmd
wget https://raw.githubusercontent.com/mlflow/mlflow/master/examples/sklearn_elasticnet_diabetes/linux/train_diabetes.py
```

- `train_diabetes.py` 파일이 생성
- `mlflow.log_param`, `mlflow.log_metric`, `mlflow.log_model`, `mlflow.log_artifact`: mlflow api를 제공해 주어 로깅이 가능

```cmd
python train_diabetes.py  0.01 0.01
python train_diabetes.py  0.01 0.75
python train_diabetes.py  0.01 1.0
python train_diabetes.py  0.05 1.0
```

- 여러 파라미터로 테스트를 하더라도 mlflow에 각 metrics와 파라미터에 따라 어떤 결과들이 나왔는지 확인이 가능
- mlruns에 생성된 여러 데이터들 확인도 가능

## MLflow 서빙

```cmd
mlflow models serve -m $(pwd)/mlartifacts/0/<run-id>/artifacts/model -p <port>
```

- 경로: MLModel이 있는 경로
- run-id: 모델을 실행하고 난 id
- `localhost:1234` API 서버가 열림
- `.predict()`함수 사용이 가능
- `localhost:1234` API 서버에 API 요청을 보내고 `predict()` 결과 확인
  ```cmd
  curl -X POST -H "Content-Type:application/json" --data '{"inputs":[[0.038076, 0.050680,  0.061696,  0.021872, -0.044223, -0.034821, -0.043401, -0.002592,  0.019908, -0.017646]]}' http://127.0.0.1:1234/invocations
  ```

## Automati Logging

```cmd
wget https://raw.githubusercontent.com/mlflow/mlflow/master/examples/sklearn_autolog/utils.py
wget https://raw.githubusercontent.com/mlflow/mlflow/master/examples/sklearn_autolog/pipeline.py
```

- MLflow에서 제공하는 example
- training 데이터를 가지고 `sklearn`의 `Pipeline`을 사용해 `StandardScaler` 전처리 이후 **LinearRegression**을 수행
- `autolog`를 통해 모델의 파라미터, metrics, model artifacts 등을 사용자가 명시하지 않아도 자동 로깅

## XGB 모델

```cmd
wget https://raw.githubusercontent.com/mlflow/mlflow/master/examples/xgboost/train.py
```

- MLflow에서 제공하는 example
- iris 데이터를 가지고 **xgboost** 모델로 classification을 수행하는 코드
- `autolog` 사용











<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps

끝!

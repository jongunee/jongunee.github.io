---
layout: post
title: 모델 Serving
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# 모델 Serving

* toc
{:toc .large-only}

## 모델 Serving이란

> ML Serving이란 학습이 끝난 ML 모델을 서비스화하는 것을 말한다

**서빙 단계의 어려움**

- 모델 개발과 소프트웨어 개발 방법의 괴리
- 모델 개발과정과 소프트웨어 개발 과정의 파편화
- 모델 평가 방식 및 모니터링 구축의 어려움

**서빙 간편화 툴**

- Seldon Core
- TFServing
- KFServing
- Torch Serve
- BentoML

## Flask

> Flask란 웹 서비스 개발을 하기 위한 프레임워크

- Django 같은 다른 프레임워크에 비해 가볍고 확장성, 유연성이 뛰어남
- 대신 자체 지원 기능은 적음

**설치**

```cmd
pip install -U Flask
```


**Flask Server**

```py
import pickle

import numpy as np
from flask import Flask, jsonify, request

# 지난 시간에 학습한 모델 파일을 불러옵니다.
model = pickle.load(open('./build/model.pkl', 'rb'))

# Flask Server 를 구현합니다.
app = Flask(__name__)


# POST /predict 라는 API 를 구현합니다.
@app.route('/predict', methods=['POST'])
def make_predict():
    # API Request Body 를 python dictionary object 로 변환합니다.
    request_body = request.get_json(force=True)

    # request body 를 model 의 형식에 맞게 변환합니다.
    X_test = [request_body['sepal_length'], request_body['sepal_width'],
              request_body['petal_length'], request_body['petal_width']]
    X_test = np.array(X_test)
    X_test = X_test.reshape(1, -1)

    # model 의 predict 함수를 호출하여, prediction 값을 구합니다.
    y_test = model.predict(X_test)

    # prediction 값을 json 화합니다.
    response_body = jsonify(result=y_test.tolist())

    # predict 결과를 담아 API Response Body 를 return 합니다.
    return response_body


if __name__ == '__main__':
    app.run(port=5000, debug=True)
```

- 아무 학습 모델을 사용해도 됨
- Flask 서버에서 predict 결과 반환

## Seldon Core

**CR(Custom Resource)**

> 쿠버네티스 커스텀 리소스은 쿠버네티스 API 확장판으로 쿠버네티스에서 기본적으로 관리하는 리소스들 외에 Custom Controller를 통해 관리되는 사용자가 직접 정의한 리소스를 말한다.

**Operator Pattern**

- Controller

  > Desired State와 Current State를 비교하여 Current State를 Desired State에 일치시키도록 지속적으로 동작하는 무한 루프

- Operator

  > Controller Pattern을 사용하여 사용자 애플리케이션을 자동화하는 것

  - Operator 개발 툴: kubebuilder, KUDO, Operator SDK

**Seldon Core 기본 환경**

- 쿠버네티스 환경(v1.18+)
  - minikube
  - kubectl
- Helm
- Ambassador

## SeldonDeployment

> SeldonDeployment는 Seldon Core에서 정의한 커스텀 리소스 중 하나이며, 이미 학습된 모델을 로드해서 서빙하는 서버를 의미한다.

**예제**

- namespace 생성

  ```cmd
  kubectl create namespace seldon
  ```

- yaml 파일 생성 후 apply

  ```yml
  apiVersion: machinelearning.seldon.io/v1
  kind: SeldonDeployment
  metadata:
    name: iris-model
    namespace: seldon
  spec:
    name: iris
    predictors:
    - graph:
        implementation: SKLEARN_SERVER # seldon core 에서 sklearn 용으로 pre-package 된 model server
        modelUri: gs://seldon-models/v1.11.0-dev/sklearn/iris # seldon core 에서 제공하는 open source model - iris data 를 classification 하는 모델이 저장된 위치 : google storage 에 이미 trained model 이 저장되어 있습니다.
        name: classifier
      name: default
      replicas: 1 # 로드밸런싱을 위한 replica 개수 (replica 끼리는 자동으로 동일한 uri 공유)
  ```

- minikube tunnel
  - minikube cluster 내부와 통신이 가능해 짐
  - ambassador와 같은 서비스를 노출시켜 externalIP 할당

**API 요청**

- Seldon External API 확인
  - API: `http://<ingress_url>/seldon/<namespace>/<model-name>/api/v1.0/doc/`

- Prediction 확인

  ```cmd
  curl -X POST http://<ambassadar IP>/seldon/seldon/iris-model/api/v1.0/predictions \
      -H 'Content-Type: application/json' \
      -d '{ "data": { "ndarray": [[1,2,3,4]] } }'
  ```

  **결과**

  ```yml
  Response:
  {'data': {'names': ['t:0', 't:1', 't:2'], 'ndarray': [[0.0006985194531162835, 0.00366803903943666, 0.995633441507447]]}, 'meta': {'requestPath': {'classifier': 'seldonio/sklearnserver:1.15.1'}}}
  ```

- Python 클라이언트

  - 패키지 설치
    ```cmd
    pip install numpy seldon-core
    ```
  - python 코드
    ```py
    import numpy as np

    from seldon_core.seldon_client import SeldonClient

    sc = SeldonClient(
        gateway="ambassador",
        transport="rest",
        gateway_endpoint="<ambassador IP>",  # Make sure you use the port above
        namespace="seldon",
    )

    client_prediction = sc.predict(
        data=np.array([[1, 2, 3, 4]]),
        deployment_name="iris-model",
        names=["text"],
        payload_type="ndarray",
    )

    print(client_prediction)
    ```
  - 위 결과와 동일하게 prediction response 반환





<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps

끝!

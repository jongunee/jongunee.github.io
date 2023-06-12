---
layout: post
title: BentoML
description: >
  [참조] https://docs.bentoml.org/en/latest/tutorial.html
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# BentoML

* toc
{:toc .large-only}

## BentoML이란?

*BentoML makes it easy to create Machine Learning services that are ready to deploy and scale.*

> 위 공식 홈페이지에서 설명하는 것과 같이 BentoML은 개발한 학습 모델을 효율적으로 배포 및 서빙할 수 있는 오픈소스이다.

BentoML 공식 홈페이지에서 제공하는 튜토리얼을 따라해보며 이해해 보기로 한다.

## BentoML 설치

**제약 조건**

- **OS:** Linux/UNIX, Windows, MacOS
- Python 3.7 이상

**설치 명령어**

```cmd
pip install bentoml
```

**Git 소스로 설치**

```cmd
pip install git+https://github.com/bentoml/bentoml
```

## tutorial 설정

1. Google Colab
    - Google Colab Notebook을 통해 샘플코드 실행
    - https://colab.research.google.com/github/bentoml/BentoML/blob/main/examples/quickstart/iris_classifier.ipynb

2. Docker
    ```cmd
    docker run -it --rm -p 8888:8888 -p 3000:3000 -p 3001:3001 bentoml/quickstart:latest
    ```

3. Local 환경
    ```cmd
    git clone --depth=1 git@github.com:bentoml/BentoML.git
    cd bentoml/examples/quickstart/
    ```

    안되면 다음 명령어로 진행한다.

    ```cmd
    git clone https://github.com/bentoml/bentoml.git
    cd bentoml/examples/quickstart/
    ```

    - 필요 dependencies 설치
    ```cmd
    pip install bentoml scikit-learn pandas
    ```

## 모델 저장

BentoML tutorial 시작을 위해 다음 파이썬 파일을 통해 모델을 저장한다.

**save_model.py**

```py
import bentoml

from sklearn import svm
from sklearn import datasets

# Load training data set
iris = datasets.load_iris()
X, y = iris.data, iris.target

# Train the model
clf = svm.SVC(gamma='scale')
clf.fit(X, y)

# Save model to the BentoML local model store
saved_model = bentoml.sklearn.save_model("iris_clf", clf)
print(f"Model saved: {saved_model}")

# Model saved: Model(tag="iris_clf:zy3dfgxzqkjrlgxi")
```

- `iris_clf`라는 이름으로 모델 생성
- 버전 자동 생성

생성된 모델은 다음과 같이 불러와서 사용 가능하다.

```py
model = bentoml.sklearn.load_model("iris_clf:2uo5fkgxj27exuqj")

# Alternatively, use `latest` to find the newest version
model = bentoml.sklearn.load_model("iris_clf:latest")
```

## 모델 Service 생성

**service.py**

```py
 import numpy as np
 import bentoml
 from bentoml.io import NumpyNdarray

 iris_clf_runner = bentoml.sklearn.get("iris_clf:latest").to_runner()

 svc = bentoml.Service("iris_classifier", runners=[iris_clf_runner])

 @svc.api(input=NumpyNdarray(), output=NumpyNdarray())
 def classify(input_series: np.ndarray) -> np.ndarray:
     result = iris_clf_runner.predict.run(input_series)
     return result
```

- 저장한 모델을 가져와 실행 가능한 형태로 변환
- 입력받은 데이터를 기반으로 예측 결과 반환

**서비스 실행**

```cmd
bentoml serve service:svc --development --reload
```

**request.py**

```py
import requests

response = requests.post(
    "http://127.0.0.1:3000/classify",
    headers={"Content-Type": "application/json"},
    json=[[5.9, 3, 5.1, 1.8]]
)

if response.status_code == 200:
    result = response.json()
    print(response.json())
else:
    print("Error:", response.text)
```

공식 홈페이지에서는 `data`라는 매개변수를 사용해 문자열로 데이터를 전송했지만 오류가 발생해 `json`으로 수정했다. 

**실행 결과**

```cmd
[2]
```
- 예측한 꽃의 품종 반환

## Bento 빌드

**bentofile.yaml**

```yaml
service: "service:svc"  # Same as the argument passed to `bentoml serve`
labels:
   owner: bentoml-team
   stage: dev
include:
- "*.py"  # A pattern for matching which files to include in the bento
python:
   packages:  # Additional pip packages required by the service
   - scikit-learn
   - pandas
```

- 식별자, 레이블 정의
- bentofile와 같은 디렉토리에 있는 모든 py 파일을 BentoML 서비스에 포함시켜 배포

**Bento 빌드**

```cmd
bentoml build
```

## 도커 이미지 생성

**생성 명령어**

```cmd
bentoml containerize iris_classifier:latest
```

**이미지 확인**

```cmd
docker images
```

**이미지 실행**

```cmd
docker run -it --rm -p 3000:3000 iris_classifier:6otbsmxzq6lwbgxi serve
```









<span style="font-size:70%">https://docs.bentoml.org/en/latest/tutorial.html</span>

끝!

---
layout: post
title: BentoML 서비스 구현
description: >
  [참조] https://docs.bentoml.org/en/latest/reference/api_io_descriptors.html
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# BentoML 서비스 구현

* toc
{:toc .large-only}

## BentoML 서비스

> BentoML 서비스는 저장되어 있는 학습 모델을 불러와 사용할 수 있도록 API 서버를 열어주는 서빙 역할을 해준다.


**Bentoml 튜토리얼 서비스 코드**

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

- 저장되어 있는 학습 모델명을 가지고 API 서버를 오픈
- 학습 모델에 따른 `input`/`output` 타입을 지정
- 입력된 데이터에 따른 모델 결과를 확인할 수 있음

## 범용적인 서비스 구현

브레인모듈 매니저가 실행시켰던 서비스 코드를 구현한다. 저장된 학습 모델, input, output 타입을 따로 지정하여 서비스를 띄울 수 있는 범용적인 서비스 구현을 목표로 한다.

**설정 파일 로드**

```py
# config 파일 로드
config_filepath = os.environ.get('CONFIG_PATH')

if config_filepath:
  with open(config_filepath, 'r') as f:
    config = json.load(f)
```

- `CONFIG_PATH`에 지정된 환경 변수를 불러와 파일 로드
- 따로 yaml 파일을 생성해서 불러오는 방식도 존재


**model runner 로드**

```py
if framework == 'sklearn':
  model_runner = bentoml.sklearn.get(model_name).to_runner()
elif framework == 'keras':
  model_runner = load_model(model_name)
elif framework == 'pytorch':
  model_runner = torch.load(model_name)
elif framework == 'tensorflow':
  model_runner = tf.saved_model.load(model_name)
else:
  raise NotImplementedError(f"Unsupported framework: '{framework}'")
```

- 프레임워크에 따라 적절한 runner를 로드

**BentoML service 생성**

```py
svc = bentoml.Service(service_name, runners=[model_runner])
```

**입력 데이터 프레임 생성**

```py
def create_input_dataframe(input_data, input_type_str):
  if input_type == 'JSON' or input_type == 'PandasSeries':
    return pd.DataFrame([input_data])
  else:
    return input_data
```

- 모델 실행을 위해 input 데이터 타입을 DataFrame 형태로 변경

**모델 실행 함수**

```py
def execute_model(input_df, framework, model_runner):
  if framework == 'sklearn':
    return model_runner.predict.run(input_df)
  elif framework == 'keras':
    return model_runner.predict(input_df)
  elif framework == 'pytorch':
    input_tensor = torch.from_numpy(input_df.to_numpy())
    with torch.no_grad():
      return model_runner(input_tensor).numpy()
  elif framework == 'tensorflow':
    return model_runner(input_df.to_numpy()).numpy()
  else:
    raise NotImplementedError(f"Unsupported framework: '{framework}'")
```

- 프레임워크별 모델 실행 부분을 구분


**output type 변환 함수**

```py
def convert_to_output_type(result, output_type):
  if output_type == 'PandasDataFrame':
    if not isinstance(result, pd.DataFrame):
      return pd.DataFrame(result)
    return result
  elif output_type == 'PandasSeries':
    if not isinstance(result, pd.Series):
      return pd.Series(result)
    return result
  elif output_type == 'NumpyNdarray':
    if isinstance(result, pd.DataFrame):
      return result.to_numpy()
    elif isinstance(result, pd.Series):
      return result.to_numpy()
  return result
```

- 모델 실행 후 예측 결과를 config 파일에서 설정한 output 타입으로 변환


**API 정의**

```py
@svc.api(input=input_adapter, output=output_adapter)
def predict(input_data):
  try:
    # 1. 입력 데이터 프레임 생성
    input_df = create_input_dataframe(input_data, input_type)

    # 2. Framework에 따라 모델 실행
    result = execute_model(input_df, framework, model_runner)

    # 3. 결과를 설정한 output type으로 변환
    output_data = convert_to_output_type(result, input_type)

    # 성공적인 응답
    if output_type == 'JSON':
      response = {
          'success': True,
          'message': 'Success',
          'result': output_data.tolist()
      }
    else:
      response = output_data

  except Exception as e:
    if output_type == 'JSON':
      # 에러 발생시 실패 응답
      response = {
          'success': False,
          'message': f"Fail: {str(e)}"
      }
    else:
      response = output_data

  return response
```

- 위에서 정의한 함수들 호출
- JSON 타입 응답 지정하여 반환






<span style="font-size:70%">[참조]https://docs.bentoml.org/en/latest/reference/api_io_descriptors.html</span>

끝!

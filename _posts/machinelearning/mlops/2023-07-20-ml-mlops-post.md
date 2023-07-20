---
layout: post
title: Brain Module Manager 구현
description: >
  [참조] https://docs.bentoml.com/en/latest/index.html
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# Brain Module Manager 구현

* toc
{:toc .large-only}

## Brain Module Manager란?

> 개발 하고자하는 브레인 모듈 매니저란 제조 현장에서 사용되는 학습 모델들을 저장 및 관리 할 수 있는 애플리케이션을 말한다.

**사용된 기술 스택**

- bentoml
- flask

## api 정의

**bentoml 모델 리스트업**

```py
@app.route('/models')
def models():
  return render_template('model_list.html', saved_models=saved_models)
```

```py
saved_models = get_saved_models(base_models_dir)
```

- `/models` 경로에 저장된 bentoml 모델 리스트업
- 저장된 모델의 모델명, 태그명 전달하여 화면 구성
- 기본적으로는 bentoml 모델을 저장하게 되면 로컬 PC의 `bentoml` 디렉토리에 모델이 저장되기 때문에 해당 디렉토리에서 모델의 리스트를 확인할 수 있음

**bentoml 서버 서빙**

```py
@app.route('/service', methods=['POST'])
def service():
  serve_file = 'service'
  port = find_available_port(2000, 3000)
  ip = '127.0.0.1'

  # body에 포함된 데이터를 'config_data'로 받기
  config_data = request.json

  # 임시 파일 생성
  config_file = tempfile.NamedTemporaryFile(delete=False)
  config_file.write(json.dumps(config_data).encode('utf-8'))
  config_file.close()

  cmd = f"bentoml serve {serve_file}:svc --host {ip} --port {port} --reload"

  # 서브 프로세스를 실행해서 환경변수로 전달
  with tempfile.NamedTemporaryFile(delete=False) as config_file:
    config_filepath = config_file.name
    with open(config_filepath, "w") as f:
      json.dump(config_data, f)
    env_vars = os.environ.copy()
    env_vars["CONFIG_PATH"] = config_filepath
    subprocess.Popen(cmd.split(), env=env_vars)

  model_name = config_data['model_name']

  if model_name not in server_info:
    server_info[model_name] = []

  server_info[model_name].append(
      {'config_data': config_data, 'ip': ip, 'port': port})
  print(server_info)

  return f"Started service: {serve_file} on port {port}"
```

- `/models` 페이지에서 모델 서빙에 필요한 `Input`과 `output` 타입을 입력 받아 임시 `json` 파일 생성
- 사용가능한 포트 정보 탐색
- 서브 프로세스를 생성해서 서빙을 실행할 때 bentoml 커맨드를 실행하며 `json` 정보를 환경변수로 전달
- bentoml 서빙 명령어 예시
  ```cmd
  bentoml serve <서비스 파일명>:<서비스 명> --host <IP> --port <port>
  ```
- 서빙을 위해 필요한 설정 정보는 변수에 저장에 두었다가 실행중인 서버 페이지를 구성할 때 사용

**활성화 중인 서버 리스트업**

```py
@app.route('/servers')
def servers():
  return render_template('server_list.html', server_info=server_info)
```

- `/service` POST 요청으로 생성된 서버의 설정 정보를 받아와 페이지 구성

## 결과 화면

**학습 모델 리스트 페이지**

![그림1](/assets/img/ml/saved_model.JPG)

- 로컬 환경에 저장되어 있는 학습 모델 리스트업
- `Framework`, `Input Type`, `Output Type` 등을 선택하고 `Run`` 버튼을 통해 모델 서빙 실행

**서빙된 서버 리스트 페이지**

![그림2](/assets/img/ml/server_list.JPG)

- 현재 띄워져있는 서빙 서버 리스트업
- 서빙된 모델의 설정 정보 및 서버 URL 확인 가능



<span style="font-size:70%">https://docs.bentoml.com/en/latest/index.html</span>

끝!

---
layout: post
title: kubeflow Pipeline
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# Kubeflow Pipeline

* toc
{:toc .large-only}

## kfp 설치

```cmd
pip install kfp
```

**설치 확인**

```cmd
kfp --help
which dsl-compile
```

## Kubeflow Pipeline 개념


**Pipeline**
- 여러 Component들을 연관성, 순서에 따라 연결지은 그래프(DAG)
- 쿠버네티스 관점에서 Workflow에 해당
- `kfp` SDK를 사용해 파이프라인을 구현하고 `dsl compiler`를 통해 쿠버네티스가 이해할 수 있는 yaml 파일로 생성하여 구현

**Component**
- 재사용 가능한 형태의 분리된 하나의 작업 단위
- 쿠버네티스 관점에서 파드에 해당
- `kfp` SDK를 사용해 Component를 구현하면 workflow yaml 파일의 `spec.templates`에 해당 컴포넌트를 감싼 부분이 추가가 됨

## Kubeflow Pipeline 예제

```py
from kfp.components import create_component_from_func

def add(value_1: int, value_2: int) -> int:
    ret = value_1 + value_2
    return ret


def subtract(value_1: int, value_2: int) -> int:
    ret = value_1 - value_2
    return ret


def multiply(value_1: int, value_2: int) -> int:
    ret = value_1 * value_2
    return ret

add_op = create_component_from_func(add)
subtract_op = create_component_from_func(subtract)
multiply_op = create_component_from_func(multiply)

from kfp.dsl import pipeline


@pipeline(name="add example")
def my_pipeline(value_1: int, value_2: int):
    task_1 = add_op(value_1, value_2)
    task_2 = subtract_op(value_1, value_2)
    task_3 = multiply_op(task_1.output, task_2.output)
```

**컴파일**

```cmd
dsl-compile --py add.py --output add_pipeline.yaml
```

해당 파일을 **Kubeflow** > **Pipelines** > **Upload pipeline**에서 파일을 업로드 후 **Runs** > **Create Run**에서 실행 

**실행 결과**

![그림1](/assets/img/ml/kf_pipeline_ex1.png)

- 각 컴포넌트가 파드 형태로 동작


위 예제 코드에 다음 코드 부분을 더해준다.

```py
if __name__ == "__main__":
    kfp.compiler.Compiler().compile(my_pipeline, "./add_pipeline_2.yaml")
```

**코드 실행**

```cmd
python add.py
```

- 따로 `dsl-compile` 명령어를 입력할 필요 없이 자동으로 결과 yaml 파일 생성  



<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps

끝!

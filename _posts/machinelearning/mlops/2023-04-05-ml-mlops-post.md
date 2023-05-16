---
layout: post
title: MLOps
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# MLOps

* toc
{:toc .large-only}

## ML 프로젝트 한계

- ML 프로젝트를 진행했을 떄 여러 학습을 시키고 나서 가장 좋았던 프로젝트를 다시 불러오는 것이 쉽게 가능할까?
- 운이 좋게 저장 했더라도 3번째로 정확했던 프로젝트를 가져오려면?
- 15번째로 정확했던 프로젝트를 가져오려면?
- 여러 명이 진행한다면 관리는?
- 개발 환경이 다르다면?

## DevOps

![그림1](https://devopedia.org/images/article/54/7602.1513404277.png)

<span style="font-size:70%">출처: https://devopedia.org/</span>

- 기존 전통적인 개발 방식: 코드 구현 > 빌드 > 배포 형태의 단방향 프로세스
- 빌드, 테스트 자동화 프로세스 추가 필요
- 배포에서 끝나는 것이 아니라 운영 중 발생하는 이슈에 대해 지속적 모니터링, 재테스트, 재배포 등을 자동화 시킬 필요성 요구
- 단방향이 아닌 지속적 라이프사이클을 가진 DevOps 방법론 탄생

**ML 프로젝트와 SW 프로젝트의 유사성**

- 버전 관리
  - 데이터 버전 관리
  - 모델 버전 관리
- 테스트 자동화
  - 모델 학습 자동화
  - 모델 성능 평가 자동화
- 모니터링
  - 서빙 모델 모니터링
  - 데이터 변화 모니터링
  - 시스템 안정성 모니터링

이러한 유사성을 가지고 있기 때문에 DevOps의 방법론을 ML에 적용시켜 MLOps 구성 가능하다.

하지만 일반적인 소프트웨어 프로젝트와 다르게 데이터의 중요성이 훨씬 크다는 차이점이 있다.

> 즉, MLOps란 ML을 효율적으로 개발하고 성공적으로 서비스화하고 운영할 때 필요한 모든 것을 다루는 분야

**MLOps 구성 요소**

- 데이터
  - 데이터 수집 파이프라인
    - 전처리, 로깅 등
    - ex) Sqoop, Flume, Kafka, Flink, Spark Streaming, Airflow
  - 데이터 저장
    - RDBMS, 분산 저장을 위한 하드, 오브젝트 스토리지 등
    - ex) MySQL, Hadoop, Amazon S3, MinIO
  - 데이터 관리
    - 데이터 Validation 체크, 버전 관리 등
    - ex) TFDV, DVC, Feast, Amundsen
- 모델
  - 모델 개발
    - 격리된 환경에서의 개발, 클라우드 환경 병렬 학습 제공 등
    - ex) Jupyter Hub, Docker, Kubeflow, Optuna, Ray, katib
  - 모델 버전 관리
    - 소스코드 형상 관리, 패키징된 형태의 모델, 모델 성능 등 기록, 지속적 CI/CD 등
    - ex) Git, MLflow, Github Action, Jenkins
  - 모델 학습 스케줄링 관리
    - GPU 인프라 관리, 스케줄링 등
    - ex) Grafana, Kubernetes
- 서빙
  - 모델 패키징
    - 의존성을 없애기 위한 Container, VM 제공
    - ex) Docker, Flask, FastAPI, BentoML, Kubeflow, TFServing, seldon-core
  - 서빙 모니터링
    - 서빙 후 환경이 잘 동작하는지 모니터링, 문제시 알람 등 자동화
    - ex) Prometheus, Grafana, Thanos
  - 파이프라인 매니징
    - 이전 모델 학습 과정을 구동하기 위해 재사용성을 위한 파이프라인 프레임워크 
    - ex) Kubeflow, argo workflows, Airflows








<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps</span>

끝!

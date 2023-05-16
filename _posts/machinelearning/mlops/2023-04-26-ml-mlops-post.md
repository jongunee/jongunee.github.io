---
layout: post
title: 모델 모니터링
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# 모델 모니터링

* toc
{:toc .large-only}

## 모델 모니터링

**발생 가능한 이슈**

- 학습된 모델을 실행하는 서버 다운
- 갑작스런 변수로 인해 모델이 정상 동작을 하지 못하는 경우

**구글 논문에 따른 ML에서 모니터링이 필요한 사항**

- 종속성 변경으로 인한 알림 발생
- 학습 및 서빙 제공 시 데이터 불변성 유지
- 학습 및 서빙이 동일한 값을 계산하는 경우
- 모델이 너무 오래되지 않은 경우
- 모델이 수치적으로 안정적인 경우
- 모델이 학습 속도, 서비스 대기 시간, 처리량 또는 RAM 사용량에서 극적으로 빠르거나 느린 누출 회귀를 경험하지 않은 경우

## 프로메테우스(Prometheus)

> SoundCloud에서 만든 모니터링 및 알림 프로그램으로 쿠버네티스와 궁합이 맞고 다양한 플러그인이 오픈소스로 제공되어 쿠버네티스 생태계에서 사실상 표준으로 볼 수 있다.

- Metric 데이터를 다차원 시계열 데이터 형태로 저장
- 데이터 분석을 위한 자체 언어 PromQL 지원
- 시계열 데이터 저장에 적합한 TimeSeries DB 지원
- 모니터링 대상의 Agent가 서버로 Metric을 보내는 Push 방식이 아닌, 서버가 직접 정보를 가져가는 Pull 방식

**구조**


![그림1](https://prometheus.io/assets/architecture.png)

<span style="font-size:70%">출처: https://prometheus.io/docs/introduction/overview/</span>

- Prometheus Server
  - 시계열 데이터 수집 및 저장
  - metrics 수집 주기를 설정이 가능하며 default 값은 15초

- Service Discovery
  - 모니터링 대상 리스트 조회
  - 사용자는 쿠버네티스에 어떤 Metrics를 조회할 것인지 등록하고 등록 후 프로메테우스 서버가 쿠버네티스 API를 통해 확인

- Exporter
  - 프로메테우스가 metrics를 수집해갈 수 있도록 HTTP 엔드포인트 제공하여 metrics를 Export
  - 프로메테우스 서버가 Exporter 엔드포인트로 Get 요청을 보내 metrics를 Pull 후 저장
  - Push 방식을 위한 Pushgateway도 제공

- Pushgateway
  - Pull 주기보다 짧은 라이프사이클을 가진 작업의 metrics 수집 보장을 위한 게이트웨이

- AlertManager
  - PromQL을 사용해 특정 조건문을 만들고 해당 조건문이 만족되면 정해진 방식

- Grafana
  - 프로메테우스와 함께 연동되는 시각화 툴

- PromQL
  - 프로메테우스가 저장한 데이터 정보를 가져오기 위한 Query Language

**Grafana 대시보드 구성**

> 수 많은 Metrics가 존재하는 데 그 중 모니터링을 위한 4가지 황금 지표는 다음과 같으며 이를 염두에 두고 대시보드를 구성하는 것을 권장한다.

- Latency: 사용자가 요청 후 응답을 받기까지의 시간
- Traffic: 시스템이 처리해야하는 총 부하
- Errors: 사용자 요청 중 실패한 비율
- Saturation: 시스템의 포화 상태

## 실습

1. minikube start
2. kube-prometheus-stack Helm Repo 추가
    ```cmd
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

    helm repo update
    ```
3. kube-prometheus-stack 설치
    ```cmd
    helm install prom-stack prometheus-community/kube-prometheus-stack
    ```
4. 포트포워딩
    - Grafana
      ```cmd
      kubectl port-forward svc/prom-stack-grafana 9000:80
      ```
    - 프로메테우스
      ```cmd
      kubectl port-forward svc/prom-stack-kube-prometheus-prometheus 9091:9090
      ```
5. 프로메테우스 실행
    - `localhost:9091` 접속
    - PromQL
      - ex) `kube_pod_container_status_running`: 동작 중인 파드 출력
      - ex) `container_memory_usage_bytes`: 컨테이너 별 메모리 사용 현황 출력
6. 그라파나 실행
    - `localhost:9000` 접속
    - 기본 접속 정보
      - ID: admin
      - Password: prom-operator
    - Prometheus가 자동으로 Data Source로 등록되어 있음
    - Dashboards - Browse 탭에서 여러 대시보드 확인 가능









<span style="font-size:70%">[참조]패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps</span>

끝!

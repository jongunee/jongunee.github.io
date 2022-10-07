---
layout: post
title: AWS EKS
description: >
  [참조]: 패스트캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# AWS EKS

* toc
{:toc .large-only}

## AWS EKS란?

> AWS EKS란 AWS의 관리형 쿠버네티스 클러스터

- AWS에서 직접 관리를 해주기 때문에 제어 영역(Control Plane)을 직접 프로비저닝하거나 관리하지 않아도 됨

**로드 밸런싱**

- Service
  - AWS ELB의 CLB(Classic Load Balancer)
로 구성
  - Annotation 설정을 통해 NLB(Network Load Balancer)로 구성 가능

- Ingress
  - aws-load-balancer-controller 추가 애드온 설치 필요
  - AWS ELB의 ALB(Application Load Balancer) 구성

**파드 네트워킹**

> CNI란 Container Network Interface의 약자로 리눅스 컨테이너 네트워크 인터페이스 설정을 위한 표준 스펙

- AWS VPC CNI는 AWS에서 제공하는 CNI Plugin
- AWS EC2는 VPC의 ENI를 통해 IP 할당
- ENI의 Secondary IP Address를 통해 노드 내 파드가 동일한 VPC IP 대역에서 파드에 IP 할당

**저장소**

- gp2 타입의 EBS 사용이 가능한 StorageClass 내장

**로그 및 메트릭**

- 클러스터 생성 시 로깅 옵션을 통해 API Server, Audit, Authenticator, Scheduler, Controller Manager에 대한 CloudWatch Logs로 수집 가능
- 노드에 대한 로그 기능은 제공하지만 Fluentd나 Fluent Bit을 통해 CloudWatch Logs로 파드 로그 전송 가이드 제공







<span style="font-size:70%">[참조]: 패스트캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.</span>

끝!

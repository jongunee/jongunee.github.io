---
layout: post
title: AWS
description: >
  [참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - devops
  - cicd
---

# AWS

* toc
{:toc .large-only}

## 클라우드 컴퓨팅

**장점**

- 언제 어디서든 접근 가능
- 원하면 언제든지 컴퓨터 자원을 늘릴 수 있음
- 사용량 기반 과금
- 초기비용이 적음
- 빠르게 전 세계에 서비스 런칭 가능

**단점**

- 관리를 위해 고급 전문 지식 필요
- 파악하기 힘든 광범위한 서비스

## AWS 주요 서비스

### 컴퓨팅 서비스

**AWS EC2(Elastic)**

> 사양과 크기를 조절할 수 있는 컴퓨팅 서비스

**AWS Lightsail**

> 가상화 프라이빗 서버

**AWS Auto Scaling**

> 서버의 특정 조건에 따라 서버 추가/삭제할 수 있게 하는 서비스

**AWS Workspaces**

> 사내 pc를 가상화로 구성하여, 문서를 개인 pc에 보관하는 것이 아니라 서버에서 보관하게 하도록 하는 서비스

### 네트워킹 서비스

**AWS Route 53**

> DNS(Domain Name System) 웹서비스

**AWS VPC**

> 가상 네트워크를 클라우드 내에 생성/구성

**AWS Diret Connect**

> On-premise 인프라와 aws를 연결하는 네트워킹 서비스

**AWS ELB**

> 부하 분산(로드 밸런싱) 서비스

### 스토리지/데이터베이스 서비스

**AWS S3**

> 여러 가지 파일을 형식에 구애 받지 않고 저장

**AWS RDS**

> 가상 SQL 데이터베이스 서비스

**AWS DynamoDB**

> 가상 NoSQL 데이터베이스 서비스

**AWS ElastiCache**

> In-memory 기반의 cache 서비스 (빠른 속도 필요로 하는 서비스오 연계)

### 데이터 분석 & AI

**AWS Redshift**

데이터 분석에 특화된 스토리지 시스템

**AWS EMR**

대량의 데이터를 효율적으로 가공 & 처리

**AWS Sagemaker**

머신 러닝 & 데이터 분석을 위한 클라우드 환경 제공

## VPC란?

> VPC란 Virtual Private Cloud의 약자로 AWS VPC를 이용하면 사용자가 정의한 가상 네트워크로 AWS 리소스를 시작할 수 있다.

- 기존 네트워크와 매우 유사
- 계정 생성 시 default로 VPC 생성
- EC2, RDS, S3 등 서비스 활용 가능
- 서브넷 구성
- 보안 설정
- VPC 간 연결
- IP 대역 지정 가능
- VPC는 하나의 Region에만 속할 수 있음

**VPC 구성 요소**

- Availability Zone
- Subnet(CIDR)
- Internet Gateway
- Network Access Control List / security group
- Route Table
- NAT(Network Address Translation) instancce / NAT gateway
- VPC endpoint

### Availability Zone

> Availability Zone이란 물리적으로 분리되어 있는 인프라가 모여 있는 데이터 센터를 말한다.

- 각 AZ는 일정 거리 이상 떨어져 있음
- 하나의 Region은 2개 이상의 AZ로 구성됨
- 각 계정의 AZ는 다른 계정의 AZ와 다른 아이디를 부여받음

### Subnet

> Subnet은 VPC의 하위 단위

- 하나의 AZ에서만 생성 가능하기 때문에 서로 다른 AZ에서 Subnet을 나눌 수 없음
- 하나의 AZ에 여러 subnet 생성 가능
- Private subnet은 인터넷에 접근 불가능한 subnet
- Public subnet은 인터넷에 접근 가능한 subnet

### IGW

> IGW란 Internet Gateway의 약자로 인터넷으로 나가는 통로를 의미한다.

- Private subnet은 IGW로 연결되어 있지 않음

### Route table

> Route table은 트래픽이 어디로 가야 할지 알려주는 테이블

- VPC 생성 시 자동으로 새성
- 10.0.0.0/16은 로컬
- 나머지는 IGW (인터넷)


<span style="font-size:70%">[참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.

끝!

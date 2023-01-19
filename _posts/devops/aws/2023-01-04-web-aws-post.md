---
layout: post
title: AWS Certified Developer Associate
description: >
  [참조] Udemy - AWS Certified Developer Associate 시험 합격을 위한 모든 것!
sitemap: true
hide_last_modified: true
categories:
  - devops
  - aws
---

# AWS Certified Developer Associate

* toc
{:toc .large-only}

## 지역 및 AZ

### AWS Regions

> AWS는 전 세계에 여러 Regions가 있으며 Region은 데이터 센터의 집합을 말한다.

- us-east-1, eu-west-3 등으로 표기
- AWS의 대부분의 서비스들은 특정 Region에 연결되어 국한  
➡️한 Region에서 어떤 서비스를 사용하다가 다른 Region에서 그 서비스를 사용하려고 하면 서비스를 처음 사용하는 셈

**Region 선택에 영향을 미칠 수 있는 요인**

- **데이터 거버넌스 및 법적 요구사항 준수:** 정부 규제 등의 이유로 애플리케이션의 데이터가 해당 국가 내에 보관되어야 하는 경우  
➡️ 프랑스 데이터는 프랑스에만 보관시키라는 규제가 있다면 프랑스 Region에 애플리케이션을 출시해야할 것
- **고객과의 근접성:** 대기 시간 단축  
➡️ 애플리케이션 사용 대부분의 고객이 미국인이라면 미국 Region에 출시하는 것이 바람직
- **지역 내 사용 가능한 서비스:** 일부 Region에서는 새로운 서비스 및 기능을 사용할 수 없음
- **요금:** 요금은 Region에 따라 다르며 서비스 요 페이지에서 확인 가능

### AWS Availability Zones

> 각 Region마다 여러 AZ(Availability Zones)가 존재한다. 일반적으로 2~6개 정도의 AZ가 존재하며 일반적으로는 3개 정도이다.

- 각 AZ는 이중화된 전원, 네트워크 및 연결을 갖춘 하나 이상의 데이터 센터를 가짐
- 각 AZ는 떨어져 있어 재난으로부터 격리됨
- 고대역폭, 초저지연 네트워킹과 연결되어 있

### AWS Points of Presence (Edge Locations)

- Amazon은 42개국 84개 도시에 216개 Points of Presence 보유
- 205개의 Edge Locations, 11개의 Regional Caches
- 최종 사용자에게 빠른 컨텐츠 전달이 가

### AWS Console

- AWS 글로벌 서비스:
  - **IAM**(Identity and Access Management)
  - **Route 53**(DNS Service)
  - **CloudFront**(Content Delivery Network)
  - **WAF**(Web Application Firewall)

- AWS 지역 서비스:
  - **Amazon EC2**(Infrastructure as a Service)
  - **Elastic Beanstalk**(Platform as a Service)
  - **Lambda**(Function as a Service)
  - **Rekognition**(Software as a Service)

글로벌 서비스인지 지역 서비스인지는 아래 그림처럼 확인 가능하다.

**글로벌 서비스**

![그림1](/assets/img/aws/check_global.png)

**지역 서비스**

![그림2](/assets/img/aws/check_region.png)

## IAM(Identity and Access Management)

> IAM은 Identity and Access Management의 약자로 글로벌 서비스 중 하나이다.

- Default로 **Root 계정**이 생성되지만 사용자를 생성하는 것 외에 사용 또는 공유해서는 안됨
- **사용자(User)**는 조직 내의 한 사람이 해당되며 필요에 따라 그룹화 시킬 수 있음
- **그룹(Group)**은 User만 포함하며 다른 그룹을 포함하지 않음
- 사용자가 그룹에 반드시 포함될 필요는 없으며 여러 그룹에 속할 수 있음

### IAM: Permissions

- **사용자 또는 그룹**은 Json 문서에 정책에 따라 권한을 부여할 수 있음
- AWS에서는 최소 권한 원칙을 적용하기 때문에 필요로 하는 것보다 더 많은 권한을 부여하지 않는 것이 원칙



<span style="font-size:70%">[참조] Udemy - AWS Certified Developer Associate 시험 합격을 위한 모든 것!

끝!

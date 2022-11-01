---
layout: post
title: VPC 기본 실습
description: >
  [참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - devops
  - aws
---

# VPC 기본 실습

* toc
{:toc .large-only}

## VPC 생성

VPC - VPC 생성 클릭

![그림1](/assets/img/aws/vpc_create.JPG)

**생성 결과**

![그림2](/assets/img/aws/vpc_create_result.JPG)

## 서브넷 생성

VPC를 생성하더라도 Default로 서브넷을 생성해 주지는 않는다.

- 자동 생성 O: 라우팅 테이블, NACL, 보안그룹 등
- 자동 생성 X: 서브넷, IGW, 엔드포인트, NAT 게이트웨이 등

VPC - 서브넷 - 서브넷 생성 클릭

**Public Subnet 생성**

![그림3](/assets/img/aws/public_subnet_create.JPG)

생성한 VPC ID를 선택, 가용 영역 선택 IP CIDR 블록 설정한다.

**Private Subnet 생성**

![그림4](/assets/img/aws/private_subnet_create.JPG)

## IGW 생성

VPC - 인터넷 게이트웨이 - 인터넷 게이트웨이 생성 클릭

![그림5](/assets/img/aws/igw-create.JPG)

**생성 결과**

![그림6](/assets/img/aws/igw-create-result.JPG)

Detached 상태인 것을 알 수 있다. 즉 어떠한 VPC에도 연결이 되어있지 않다는 의미이다.

해당 IGW를 선택 후 작업 - VPC에 연결 클릭

![그림7](/assets/img/aws/igw-connect.JPG)

생성했던 VPC ID를 선택해 준다.

**연결 결과**

![그림7](/assets/img/aws/igw-connect-success.JPG)

## 라우팅 테이블 생성

**Public 라우팅 테이블 생성**

![그림8](/assets/img/aws/public_rtb_create.JPG)

생성했던 VPC ID를 선택해 준다.

**Private 라우팅 테이블 생성**

![그림9](/assets/img/aws/private_rtb_create.JPG)

VPC 생성으로 자동 생성된 Default 라우팅 테이블의 이름을 바꿔준다.

**서브넷 연결**

Public 라우팅 테이블 선택 후 작업 - 서브넷 연결 편집 클릭

![그림10](/assets/img/aws/public_rtb_subnet_connect.JPG)

**연결 결과**

![그림11](/assets/img/aws/public_rtb_subnet_connect_reseult.JPG)

Public 서브넷과 Public 라우팅 테이블이 잘 연결된 것을 확인할 수 있다. 

![그림12](/assets/img/aws/private_rtb_subnet_connect.JPG)

Private 라우팅 테이블도 Public 라우팅 테이블과 동일하게 Private Subnet을 선택, 편집해도 동작하지만 그림처럼 특별한 설정이 없다면 기본 Subnet에 연결이 되기 때문에 설정해 줄 필요가 없다.

**Public 라우팅 테이블 설정**

라우팅 테이블 선택 후 라우팅 - 라우팅 편집 클릭

![그림13](/assets/img/aws/public_rtb_configure.JPG)

라우팅 추가 후 생성했던 인터넷 게이트웨이와 연결해 준다.

라우팅 테이블은 위에서부터 적용이 되기 때문에 `10.0.0.0/16` 이외의 IP 영역에서만 인터넷 게이트웨이로 연결된다.

**Private 라우팅 테이블 설정**

Private 라우팅 테이블은 `10.0.0.0/16` 이외의 IP 영역은 차단해주면 되기 때문에 별도의 설정이 필요 없다.



<span style="font-size:70%">[참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.

끝!

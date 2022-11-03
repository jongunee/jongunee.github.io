---
layout: post
title: NAT Gateway와 VPC Endpoint
description: >
  [참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - devops
  - aws
---

# NAT Gateway와 VPC Endpoint

* toc
{:toc .large-only}

## NAT Gateway 생성

NAT 게이트웨이 - NAT 게이트웨이 생성 클릭

![그림1](/assets/img/aws/nat_gateway_name.JPG)

NAT 게이트웨이 이름을 입력, 탄력적 IP 주소를 할당해주고 서브넷은 public subnet 선택해 준다.

## 라우팅 테이블 편집

**Private 라우팅 테이블 설정**

![그림2](/assets/img/aws/private_rtb_add_nat.jpg)

Private 라우팅 테이블에 생성한 NAT 게이트웨이를 대상으로 추가해준다.

이를 통해 Private 인스턴스에서 인터넷 접근이 가능해진다.


## VPC Endpoint 생성

**역할 생성**

IAM - 역할 - 역할 생성 버튼 클릭

![그림3](/assets/img/aws/vpc_ep_role.jpg)

EC2 사례를 선택해준다.

![그림4](/assets/img/aws/vpc_ep_policy.jpg)

S3에 대한 모든 접근이 가능한 `AmazonS3FullAccess` 권한을 추가한다.

![그림5](/assets/img/aws/vpc_ep_role_name.jpg)

![그림6](/assets/img/aws/vpc_ep_role_tag.jpg)

이름과 태그도 추가해준다.

## Private 인스턴스 생성

![그림7](/assets/img/aws/vpc_ep_ec2_name.jpg)

이름을 설정해준다.

**네트워크 설정**

![그림8](/assets/img/aws/vpc_ep_ec2_network.jpg)

생성해 둔 VPC, Private 서브넷으로 설정한다.

**보안 그룹 설정**

![그림9](/assets/img/aws/vpc_ep_ec2_sg.jpg)

보안 그룹은 기존에 생성해 두었던 private 보안 그룹으로 설정

## Public 인스턴스 생성

이전 포스팅과 동일하게 생성해준다.

## S3 생성

S3 - 버킷 - 버킷 만들기 버튼을 클릭

![그림10](/assets/img/aws/s3_bucket.jpg)

S3 버킷의 이름만 설정해주고 생성한다.

**Private 라우팅 테이블 수정**

![그림11](/assets/img/aws/private_rtb_edit.jpg)

확인을 위해 라우팅 테이블을 수정한다.

## 엔드포인트 생성

VPC - 엔드포인트 - 엔드포인트 생성 클릭

![그림12](/assets/img/aws/ep_name.jpg)

이름을 설정해준다.

**ep 서비스 설정**

![그림13](/assets/img/aws/s3_service.jpg)

S3를 검색해서 해당 서비스를 선택한다.

**VPC 설정**

![그림14](/assets/img/aws/ep_vpc_choice.jpg)

생성했던 VPC를 선택한다.

**라우팅 테이블 설정**

![그림15](/assets/img/aws/ep_rtb.jpg)

라우팅 테이블은 Private 라우팅 테이블로 설정한다.




<span style="font-size:70%">[참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.

끝!

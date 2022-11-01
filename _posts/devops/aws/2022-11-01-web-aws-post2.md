---
layout: post
title: NACL 설정
description: >
  [참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - devops
  - aws
---

# NACL 설정

* toc
{:toc .large-only}

## NACL과 Security Group 차이

- NACL
  - Stateless 방식
  - 상태를 기억하지 않아 규칙에 따라서만 동작
  - 요청 정보를 따로 저장하지 않기 떄문에 응답하는 트래픽도 제어 해주어야 함
- SG
  - Stateful 방식
  - 상태를 기억하는 방식
  - 요청 정보를 저장하여 응답하는 트래픽 제어가 필요 없음

## NACL 생성

**Public NACL 생성**

네트워크 ACL - 네트워크 ACL 생성 버튼 클릭

![그림1](/assets/img/aws/public_nacl_create.JPG)

이름 설정, 생성된 VPC로 설정 후 생성해준다.

**Private NACL 생성**

VPC 생성시 생성되는 Default NACL을 Private으로 이름만 바꾸어준다.

**생성 결과**

![그림2](/assets/img/aws/nacl_result.JPG)

## 인/아웃바운드 규칙 설정

**Public NACL 인바운드 규칙 설정**

Public NACL 선택 후 인바운드 규칙 - 인바운드 규칙 편집 클릭

![그림3](/assets/img/aws/public_nacl_inbound.JPG)

이와같이 포트를 설정해준다.

규칙 번호가 낮은 순에 따라 적용이 되기 때문에 같은 범위라면 낮은 규칙 번호에 해당하는 규칙만 적용이 된다.

**Public NACL 아웃바운드 규칙 설정**

Public NACL 선택 후 아웃바운드 규칙 - 아웃바운드 규칙 편집 클릭

![그림4](/assets/img/aws/public_nacl_outbound.JPG)

이와같이 포트를 설정해준다.

response는 Request와 다르게 임시 포트에서 이루어지기 때문에 임시포트의 범위에 해당하는 1024 ~ 65535로 설정해 준다.

## 인/아웃바운드 규칙 적용 확인

**Public NACL 인바운드 규칙 설정**

Public NACL 선택 후 인바운드 규칙 - 인바운드 규칙 편집 클릭

![그림5](/assets/img/aws/public_nacl_inbound_allow.JPG)

편의를 위해 우선 포트를 모두 허용한다.

**VPC Public Subnet 자동 할당 IP 설정**

VPC - 서브에서 Public subnnet 선택 후 작업 - 서브넷 설정 편집 클릭

퍼블릭 IPv4 주소 자동 할당 활성화 체크


## EC2 생성

EC2 - 인스턴스 시작 버튼 클릭

![그림6](/assets/img/aws/public_ec2_name.JPG)

이름 설정을 해준다.

**인스턴스 설정**

![그림7](/assets/img/aws/pulic_ec2_instance.JPG)

인스턴스는 무료 tier인 t2.micro로 생성한다.

**네트워크 설정**

![그림8](/assets/img/aws/public_ec2_vpc_connect.JPG)

VPC와 서브넷을 설정해주고 퍼블릭 IP 자동 할당 활성화 해준다.

고급 세부 정보 - 사용자 데이터에 코드 작성

![그림9](/assets/img/aws/public_ec2_user_data.JPG)

**보안 그룹 설정**

![그림10](/assets/img/aws/public_ec2_sg.JPG)

위와 같이 보안 그룹 설정을 해준다.

**키 페어 설정**

생성된 키 페어가 없다면 새로 생성

![그림11](/assets/img/aws/key_pair_create.JPG)




<span style="font-size:70%">[참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.

끝!

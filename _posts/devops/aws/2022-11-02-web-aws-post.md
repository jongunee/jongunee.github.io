---
layout: post
title: Bastion Host
description: >
  [참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - devops
  - aws
---

# Bastion Host

* toc
{:toc .large-only}

## Public 인스턴스 생성

**이름 설정**

![그림1](/assets/img/aws/bas_host_name.JPG)


**네트워크 설정**

![그림2](/assets/img/aws/bas_host_vpc.JPG)

기존 생성했던 VPC, Public 서브넷을 선택해준다.

**스토리지 구성**

![그림3](/assets/img/aws/bas_host_storage.JPG)

스토리지는 마그네틱(표준)으로 선택해준다.

**보안 그룹 설정**

![그림4](/assets/img/aws/bas_host_sg.JPG)

이후 인스턴스를 생성해준다.

## Private 인스턴스 생성

**이름 설정**

![그림5](/assets/img/aws/bas_private_ec2_name.JPG)

**네트워크 설정**

![그림6](/assets/img/aws/bas_private_ec2_vpc.JPG)

기존 생성했던 VPC, Private 서브넷을 선택해주고 퍼블릭 IP 자동 할당을 비활성화 시켜준다.

**스토리지 구성**

![그림7](/assets/img/aws/bas_private_ec2_storage.JPG)

**보안 그룹 설정**

![그림8](/assets/img/aws/bas_private_ec2_sg.JPG)

보안 그룹의 Source는 위에서 생성했던 public ec2의 보안그룹(public-sg-test)을 선택한다.

## Public 인스턴스 접속

Private 인스턴스에 접근하기 위해 우선 Public 인스턴스에 접속해준다.

![그림9](/assets/img/aws/bas_public_access.png)

MobaXterm을 사용을 했고 Putty를 사용해도 무관하다.

설정:
- SSH 선택
- Public ec2 IP주소 입력
- 다운받은 키페어 선택
- username: ec2-user로 접속

**Private 인스턴스 정보 확인**

![그림10](/assets/img/aws/bas_private_ec2_info.png)

Private 인스턴스 정보를 보고 프라이빗 IPv4 주소를 확인한다.

## Private 인스턴스 접속

```bash
ssh <Private 인스턴스 IP 주소>
```



<span style="font-size:70%">[참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.

끝!

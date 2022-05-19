---
layout: post
title: OPC UA 개요
description: >
  OPC UA에 대해
sitemap: true
categories:
  - certification
  - opcua
---

## OPC UA 개요

* toc
{:toc .large-only}

## OPC UA란?

![그림1](/assets/img/opcua/opcua.png)
>_Open Platform Communication Unified Architecture_ 의 약자로 <span style='background-color: #f5f0ff'>상호운용성</span>이 뛰어나며 현재 OT 환경에서 많이 쓰이는 <span style='background-color: #f5f0ff'>산업용 표준 프로토콜</span>

---

## 기본 OT

__자동화 피라미드 - 퍼듀 모델__

![그림2](/assets/img/opcua/automation%20pyramid.jpg){: width="500px" height="335px"}

[참조] The Advantech automation system pyramid(2013/12/17) <https://processengineering.co.uk/article/2017704/the-automation-syste>

- Level 0: Field Level
  - 센서 값 측정, 액츄에이터 작동
  - ex) 각종 센서 및 액츄에이터
- Level 1: Process Control Level
  - 변수 제어를 담당하는 프로세스의 브레인
  - ex) PLC, DCS
- Level 2: Process Management Level
  - 사용자 인터페이스를 통해 프로세스 데이터 모니터링을 하며 데이터베이스에 저장
  - ex) SCADA, GMI
- Level 3: Operation Level
  - Level 2에서 발생한 데이터를 공정, 생산 데이터 등의 유용한 정보로 변환
  - ex) MES
- Level 4: Enterprise Level
  - 급여, 원자재, 구매 주문서 등 모든 데이터를 가지며 사업 전체 관리 (Business Intelligence 담당)
  - ex) ERP

__CPS 모델__

![그림3](/assets/img/opcua/cps.jpg)

[참조] The Advantech automation system pyramid(2013/12/17) <https://cpsiot.at/cps-iot-ecosystem-a-platform-for-research-and-education/193/>

> CPS란? _Cyber Physical Systems의 약자로 우리가 살아가는 물리적 세계와 컴퓨터, 통신, 인터넷과 같은 사이버 세계가 융합된 형태를 말한다._ CPS를 통해 물리적 세계와 사이버 세계가 상호 작용을 하면서 새로운 시스템이 구축될 것이다.


 __상호운용성__

> 상호운용성이란? _하나의 시스템이 동일 또는 이기종의 다른 시스템과 아무런 제약이 없이 서로 호환되어 사용할 수 있는 성질을 말한다._ <span style='background-color: #f5f0ff'>상호운용성</span>이 좋다는 것은 공정에서 기기들이 잘 호환되어 프로토콜이 다르더라도 연결이 용이하고 정확한 정보 교환 및 처리가 가능하다는 것을 의미한다. 

따라서 스마트팩토리를 구현하기 위해서 각종 기기간의 호환은 필수이기 때문에 <span style='background-color: #f5f0ff'>상호운용성</span>은 굉장히 중요한 성질이다.

- 스마트팩토리에서 AGV 로봇은 제품이 언제 생산 되었는지, 어떤 제품이 생산이 되었는지 생산 데이터를 알고 연동되어야 하기 때문에 상호운용성이 중요하다.
-  SI 업체가 시스템을 통합 할 때 생산 장비 제조사가 다르게 되면 다른 프로토콜로 통신을 하게되어 상호운용성 문제가 생긴다.
- OPC UA 같은 표준 프로토콜이 없다면 생산 장비를 모두 같은 제조사로 통일시키거나 HMI 같은 값비싼 통신 인터페이스 프로그램을 사용한다.

---

## OPC Classic

>일반적으로 OPC UA가 아닌 OPC라 함은 OPC Classic을 의미한다. OPC Classic에서 OPC는 _OLE for Process Control_ 의 약자로 공정 제어에서 쓰이는 OLE를 의미한다.

- Windows OS에서만 지원
- OPC DA, AE, HDA로 분리된 아키텍처
  - OPC DA: 데이터 Read/Write 등의 교환 정의
  - OPC AE: 이벤트 발생에 따라 알람 발생
  - OPC HDA: 데이터 저장, 과거 데이터 쿼리
- 보안에 대한 정의 X

OLE는 마이크로소프트웨어에서 개발한 DCOM을 이용한 통신 기술을 말하기 때문에 OPC Classic은 Windows OS에 한정적이었다고 말할 수 있다.

---

## OPC UA - OPC Classic 단점 보완

- Windows뿐만 아니라 Lunux, Mac 등 대부분의 OS에서 지원
- OPC DA, AE, HDA로 나뉘었던 아키텍처를 하나의 단일 OPC UA 서버에서 동작하도록 강화
- 보안 규격 정의
- IEC 62541 규격으로 표준화

_Open Platform Communication Unified Architecture_ 의 약자로 OPC UA에서 많은 것들이 보안되면서 OPC Classic에서의 OPC 의미 자체가 달라진 것을 알 수 있다.

끝!
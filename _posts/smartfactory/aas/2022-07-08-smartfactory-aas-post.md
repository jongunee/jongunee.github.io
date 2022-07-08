---
layout: post
title: AAS란?
description: >
  test
sitemap: true
hide_last_modified: true
categories:
  - smartfactory
  - aas
---

# AAS란?

- toc
{:toc .large-only}

## AAS 필요성

1. 대다수 제조 업체가 디지털 솔루션에 투자 중
   - 스마트팩토리 구현 목표
2. 상호운용성 이슈
   - 개발한 솔루션이 다른 벤더 시스템과 호환이 될지는 미지수
3. 다양한 시스템 자산 정보 통합
   - PLM, ERP, CRM, MES 등

## Asset

> Asset이란 스마트팩토리 솔루션과 연결이 필요한 모든 것을 의미한다.

- ex) 장비, 컴포넌트, 재료, 부품, 교환 문서(설계도 등), 계약서, 주문 정보 등

## AAS(Asset Administration Shell)

> AAS란 디지털 세계에 구현된 Asset들의 정보 및 기능들을 관리하기 위해 고안된 일종의 프로파일

- 자산 표준화
- 자산을 표현하는 디지털 표현 ➡️ 디지털 트윈
- 자산을 명확히 식별 하는 것이 가능
- 특성, 속성, 상태, 매개변수, 특정 데이터, 기능 등 포함
- 여러 기능을 Submodel을 구성해서 표현
- 다른 통신 채널 및 애플리케이션과의 연결 링크 역할

## AAS Layer

![그림1](/assets/img/aas/aas_layer.png)

- 현재는 UML 모델이 아니라 AASX 스키마가 따로 존재

**IEC 61360 / ISO 13584-42 / ECLASS**

- 표준 데이터 형식
- 데이터 교환의 기본 디스크립션 정의
- Properties 정의
- IEC 전기전자 용어사전

**AutomationML**

- xml 스키마 기반 데이터 포멧
- 엔지니어링 툴 간 데이터 교환을 위해 개발
- 확장성 Good
- 정적 데이터 처리에 수월

**XML, JSON**

- IT 시스템에 정보 추출/제공
- JSON 형식은 AAS에 대한 http 또는 REST API 데이터를 설명하는 데 사용

**RDF**

- 데이터 모델링 표시
- W3C 권장 표준
- 그래프로 해석
- URI를 사용하여 인코딩 ➡️ 웹 표준과 밀접 관련

**OPC UA Information Model**

- IEC 62541
- 산업용 데이터 통신 프로토콜
- 기기간 상호운용성
- Information model
- 동적 데이터

## AAS 구조

![그림2](/assets/img/aas/aas_structure.png)

참조 문서: [Details of the Asset Administration Shell - Part 1](https://www.plattform-i40.de/IP/Redaktion/EN/Downloads/Publikation/Details_of_the_Asset_Administration_Shell_Part1_V3.html)

끝!

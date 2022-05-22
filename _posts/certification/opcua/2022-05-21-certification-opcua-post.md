---
layout: post
title: OPC UA 특성 및 목표
description: >
  OPC UA에 대해
sitemap: true
categories:
  - certification
  - opcua
---

## OPC UA 특성

* toc
{:toc .large-only}

## OPC UA 목표

>OPC UA가 어떤 목표를 가지고 만들어진 프로토콜인지를 말한다. 

1. Functional Equivalence:  
OPC Classic에서 제공하는 규격인 OPC DA, AE, HDA 등이 모두 OPC UA에 적용될 수 있도록 할 것
2. Platform Independence:  
한 가지 플랫폼에 종속적인 것이 아니라 임베디드 MCU부터 클라우드 기반 인프라까지 모든 플랫폼에 독립적일 것
3. Security:  
OPC Classic의 취약한 보안을 강화할 것
4. Extensible:  
기존 애플리케이션에 영향을 주지않고 새로운 기능을 추가할 수 있도록 할 것
5. Comprehensive Information Modelling:  
정보 모델링을 통해 복잡한 Multi layer 구조도 정의 가능하도록 할 것

---

## Industry 4.0의 요구사항과 OPC UA 솔루션

Industry 4.0에서의 몇 가지 요구사항들과 OPC UA가 어떻게 이 요구사항들을 충족하는지 설명한다.

|  | Industry 4.0 | OPC UA |
| --- | --- | --- |
| Independece | 독립성을 갖출 것 | 대부분의 프로그래밍 언어와 OS로 구현 가능 |
| Vertical & Horizontal Integration | 수평 및 수직접 통합이 가능할 것 | 상호운용성을 통해 증명 |
| Security | 안전한 보안 기능 | X.509 인증서를 통해 보안 안전 증명 |
| Standardization | 확립된 표준을 통한 통신 요구 | TCP 기반 통신 |
| Information Modelling | 객체 모델링 제공 | 객체 지향의 Address Space 개념 제공 |
| Verification | 검증 가능할 것 | IEC62541 표준으로 제정 |

---

## OPC UA 관련 표준

OPC UA Specification은 IEC 규격으로 제정되어 있다.
- 15개의 규격이 IEC 표준으로 제정  
➡ IEC62541-1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 100
- 10개의 규격이 KS 표준으로 제정  
➡ KS C IEC62541-1, 2, 3, 4, 5, 6, 7, 8, 9, 10
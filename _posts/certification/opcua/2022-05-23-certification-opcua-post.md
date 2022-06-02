---
layout: post
title: OPC UA 규격
description: >
  OPC UA에 대해
sitemap: true
categories:
  - certification
  - opcua
---

# OPC UA 규격

* toc
{:toc .large-only}

## OPC UA 규격이란?

>OPC UA에서 제공하는 Spec들을 파트별로 나누어 문서화시킨 것으로 크게 Core, Access Type, Utility Type 파트로 구분된다.

![그림1](/assets/img/opcua/specification.png)

- Core Specification Parts  
  - __Part 1: Overview & Concepts__  
  OPC UA의 전반적인 개요  

  - __Part 2: Security Model__  
  하드웨어 및 소프트웨어 환경 보안 위협 설명 및 응용 프로그램 개발시 해결해야 할 보안  

  - __Part 3: Address Space Model__  
  OPC UA에서 제공하는 Address Space 개념

  - __Part 4: Services__  
  OPC UA 서버와 클라이언트 간의 상호 작용을 의미하는 OPC UA Service 개념

  - __Part 5: Information Model__  
  Part 3와 연관지어 Address Space의 표준화된 노드 설명

  - __Part 6: Service Mappings__  
  추상적인 규격과 구현을 위한 기술 사이 매핑 정의

  - __Part 7: Profiles__  
  OPC UA 프로파일 정의 및 종류

  - __Part 14: PubSub__  
  일반적인 서버-클라이언트 모델이 아닌 Publisher - Subscriber 모델 설명 

- Access Type Specification Parts  
  - __Part 8: Data Access__  
  데이터 접근에 필요한 노드 클래스, Type, Arrtibute, 동작 등 정의

  - __Part 9: Alarm & Conditions__  
  Event 기반 Alarm & Conditions 모델 정의

  - __Part 10: Programs__  
  프로그램과 관련된 노드 클래스, Method, Event 등 정의

  - __Part 11: Historical Access__  
  과거 데이터 접근에 필요한 노드 클래스, Type, Arrtibute, 동작 등 정의

- Utility Specification Parts:
  - __Part 12: Discovery__  
  OPC UA 탐색 메커니즘, 연결 방식 설명
  
  - __Part 13: Aggregates__  
  데이터 집계 정의


끝!
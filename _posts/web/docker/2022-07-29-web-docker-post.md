---
layout: post
title: 쿠버네티스
description: >
  인프런 - 쉽게 시작하는 쿠버네티스(v1.20) 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# 쿠버네티스

* toc
{:toc .large-only}

## 쿠버네티스란?

> 도커와 같은 컨테이너 관리용 오케스트레이션으로 가장 빠르게 발전하는 사실상 표준(De facto)  
> Kubernetes는 k8s로 표현하기도 함

구글에서 사용하던 Borg 시스템을 CNCF 재단에 기부해 CNCF가 관리하고 있다 

## 쿠버네티스 배포 종류

1. 관리형 쿠버네티스
    - 사용자가 관리할 필요가 없고 필요한 어플리케이션을 올려 배포만 하면됨 
    - ex) aws, Azure, Google Cloud 등
2. 설치형 쿠버네티스
    - 설치를 할 수 있도록 패키지화한 것
    - ex) Rancher, OpenShift
3. 구성형 쿠버네티스
    - 필요로 하는 환경 내에서 자유롭게 구성하고자 하거나 교육이 목적인 경우
    - ex) kuberadm, kops, Kuberspray, KRIB 등

## 개발 환경

1. Vagrant
    - 다운로드: <https://www.vagrantup.com/downloads>
2. Virtual box
    - 다운로드: <https://www.virtualbox.org/>
3. Kubernetes

<span style="font-size:70%">[참조] 인프런 - 쉽게 시작하는 쿠버네티스(v1.20)

끝!

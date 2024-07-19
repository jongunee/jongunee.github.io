---
layout: post
title: A Novel Approach to Industrial Defect Generation through Blended Latent Diffusion Model with Online Adaptation
description: >
  [참조]<https://github.com/GrandpaXun242/AdaBLDM/>
sitemap: false
hide_last_modified: true
categories:
  - til
  - papers
---

# AdaBLDM

* toc
{:toc .large-only}

## Abstract

이 논문은 <span style='background-color: #f5f0ff'>산업 결함 탐지(Anomaly Detection, AD)</span>을 개선하기 위한 새로운 알고리즘인 AdaBLDM을 소개한다. 산업 결함 탐지를 효과적으로 수행하기 위해서는 많은 결함 샘플들이 필요하기 때문에 높은 품질의 결함 샘플을 생성하기 위한 생성형 AI 모델을 설명하고 있다.

## Introduction

### AD 알고리즘

- 제조 공정에서 결함이 있는 샘플을 얻는 것은 결함이 없는 샘플을 얻는 것보다 어려움
- 대부분의 AD 알고리즘은 결함이 없는 샘플로 학습을 하고 해당 분포에 속하지 않는 샘플을 이상치로 간주
- 하지만 결함이 있는 샘플을 포함시키면 정확도가 크게 향상되지만 이는 데이터 분포 불균형으로 이어짐
- 실제와 같은 결함이 있는 샘플을 생성하는 것이 목표

### 개선 사항

- BLDM(Blended Latent Diffusion Model) 활용
- Trimap을 언어 프롬프트와 함께 사용

## Related Work

### Synthetic Defect Generation

- 결함 모방 방식:
  - 정상 이미지에 이상 픽셀을 붙여 결함 생성
  - 새로운 결함 패턴을 생성할 수 없으며 과적합 문제 발생 가능성이 높음
  - ex) Crop&Paste, PRN

- 이상 패턴 생성 방식:
  - 데이터셋에 Noise를 결합하여 결함 생성
  - 생성된 결함의 분포가 실제 결함과 다를 수 있어 성능 향상 보장이 안됨
  - ex) DRAEM, DeSTeg, MegSegm ReSynthDetect

- GAN(Generative Adversarial Networks):
  - SDGAN: 두 개의 생성기를 사용하여 결함과 비결함 상태를 전환해 고품질 이미지 생성
  - Defect-GAN: 비결함 이미지를 결함 이미지로 변환하여 결함 분류기 학습
  - 결함의 위치가 부정확할 수 있음
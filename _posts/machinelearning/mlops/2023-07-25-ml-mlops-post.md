---
layout: post
title: Yatai
description: >
  [참조] https://docs.yatai.io/en/latest/installation/index.html
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# Yatai

* toc
{:toc .large-only}

## Yatai 설치

> Yatai란? BentoML에서 제공하는 학습 모델들을 관리해주는 서비스이다. 쿠버네티스 환경에서 학습 모델을 서빙, 배포 등이 가능하다.

AWS 환경에서 EKS 클러스터에 구축해 보기로 한다.

**Prerequisites**

- 쿠버네티스: v1.20 이상의 쿠버네티스 클러스터 필요
- Dynamic Volume Provisioning
- Helm
- jq
- AWS CLI

설치는 yatai 공식 설치 홈페이지를 따라 설치한다. [링크](https://docs.yatai.io/en/latest/installation/index.html)

**postgresql 연결 에러**

```cmd
kubectl -n yatai-system delete pod postgresql-ha-client 2> /dev/null || true; \
kubectl run postgresql-ha-client --rm --tty -i --restart='Never' \
    --namespace yatai-system \
    --image docker.io/bitnami/postgresql-repmgr:14.4.0-debian-11-r13 \
    --env="PGPASSWORD=$PG_PASSWORD" \
    --command -- psql -h $PG_HOST -p $PG_PORT -U $PG_USER -d $PG_DATABASE -c "select 1"
```

해당 명령어로 테스트를 할 때 서버 연결 에러가 발생했다.

**해결 방법:**

- postgreSQL VPC와 쿠버네티스 VPC를 동일하게 맞춰주거나 두 VPC 피어링 연결을 해준다.
- postrgreSQL의 포트를 열어준다.(TCP 5432)



<span style="font-size:70%">[참조]https://docs.yatai.io/en/latest/installation/index.html</span>

끝!

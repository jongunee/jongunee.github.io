---
layout: post
title: Jenkins Github 연동
description: >
  [참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.
sitemap: true
hide_last_modified: true
categories:
  - devops
  - cicd
---

# Jenkins Github 연동

* toc
{:toc .large-only}

## Jenkins

> Jenkins란 빌드, 테스트 및 배포와 관련된 소프트웨어 개발 부분을 자동화함으로써 <span style='background-color: #f5f0ff'>지속적인 통합(CI)</span>과 <span style='background-color: #f5f0ff'>지속적인 배포(CD)</span>가 가능한 오픈 소스

## item 생성

**새로운 Item** 버튼 클릭

![그림1](/assets/img/cicd/new_item.jpg)

이름 설정 및 파이프라인 선택

![그림2](/assets/img/cicd/item_general.jpg)

GitHub project를 선택 후 CICD 프로젝트에 사용될 Github 레포지터리 URL 입력 (맨 뒤 .git 제거)

![그림3](/assets/img/cicd/general_build_triggers.jpg)

Build Triggers는 Github hook trigger 선택

![그림4](/assets/img/cicd/pipeline_definition.png)

위 그림과 같이 설정하며 **Add** 버튼을 클릭하여 **Jenkins** 선택 

![그림5](/assets/img/cicd/pipeline_credentials.png)

Github Username과 Password/Accesstoken을 설정하여 생성한다. Accesstoken을 사용시에는 repo와 repo_hook을 활성화 해준다.

![그림6](/assets/img/cicd/script_path.jpg)

마지막으로 Git 레포지터리에 업로드되어 있는 소스코드의 jenkins 파일 위치를 설정 후 저장




<span style="font-size:70%">[참조] 패스트 캠퍼스 - 한 번에 끝내는 AWS 인프라 구축과 DevOps 운영 초격차 패키지 Online.

끝!

---
layout: post
title: DVC
description: >
  [참조] 패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps
sitemap: true
hide_last_modified: true
categories:
  - machinelearning
  - mlops
---

# DVC

* toc
{:toc .large-only}

## Data Management

머신러닝 서비스를 구축하게 되면 일반적인 소프트웨어 프로젝트와 다르게 대용량의 데이터 저장소를 필요로 하게 된다.

- Git LFS
- Git + Cloud Service(Google Drive, S3 등)

이와 같이 대용량 스토리지를 연동해서 사용할 수 있지만 불편함이 생겨 소스 코드와 데이터 버전을 동시에 관리할 수 있는 툴들이 등장한다.

- DVC
- Pachyderm
- Delta Lake
- Dolt

## DVC(Data Version Control)

- 대부분의 스토리지와 호환
- Github, GitLab, Bitbucket 등 git 호스팅 서버와 연동
- 전처리 코드들을 포함한 데이터 파이프라인을 DAG로 관리 가능
- Git과 유사한 인터페이스

## DVC 실행

**설치**

- 파이썬 설치
- git 설치
- dvc 설치
  ```cmd
  pip install dvc
  ```

**저장소 세팅**

- 원하는 경로로 이동
- 해당 경로 git 저장소로 초기화
  ```cmd
  git init
  ```
- 해당 경로 dvc 저장소로 초기화
  ```cmd
  dvc init
  ```

**Push**

- data 경로에 data(demo.txt) 생성
- dvc로 tracking
  ```cmd
  dvc add data/demo.txt
  ```
- demo.txt.dvc 파일 생성
- git commit
- 구글 드라이브 폴더 경로 id 세팅
  ```cmd
  dvc remote add -d storage gdrive://<구글 드라이브 폴더 ID>
  ```
- dvc config git에 commit
- 데이터 remote storage(구글 드라이브 폴더)에 업로드
  ```cmd
  dvc push
  ```

- 만약 아래 에러가 발생한다면

  ```cmd
  ERROR: unexpected error - gdrive is supported, but requires 'dvc-gdrive' to be installed: No module named 'dvc_gdrive'
  ```

  다음 패키지를 설치

  ```cmd
  pip install dvc[gdrive]
  ```
- 구글 드라이브에 업로드가 정상적으로 되었고 파일을 다운받아도 동일한 파일임을 확인 가능

**Pull**

- 기존 dvc 캐시 삭제
  ```cmd
  rm -rf .dvc/cache/
  ```
- pull 확인을 위해 기존 데이터 삭제
  ```cmd
  rm -rf data/demo.txt
  ```
- 데이터 가져오기
  ```cmd
  dvc pull
  ```

**버전 변경**

- 데이터 변경
  ```cmd
  vim data/demo.txt
  ```
- add해서 .dvc 파일 변경
  ```cmd
  dvc add data/demo.txt
  ```
- git에도 add 후 commit
  ```cmd
  git add data/demo.txt.dvc
  ```

  ```cmd
  git commit -m "메시지"
  ```
- push

  ```cmd
  dvc push
  ```

  ```cmd
  git push
  ```

**Checkout**

- git 로그에서 <COMMIT_HASH> 확인
  ```cmd
  git log --oneline
  ```

- demo.txt.dvc 파일을 이전 버전으로 되돌림
  ```cmd
  git checkout <COMMIT_HASH> data/demo.txt.dvc
  ```

- demo.txt.dvc 내용을 확인하고 demo.txt 파일을 이전 버전으로 변경
  ```cmd
  dvc checkout
  ```









<span style="font-size:70%">패스트캠퍼스 - 머신러닝 서비스 구축을 위한 실전 MLOps

끝!

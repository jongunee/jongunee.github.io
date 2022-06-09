---
layout: post
title: 웹서비스 런칭
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: false
categories:
  - til
  - web
---

# 웹서비스 런칭

* toc
{:toc .large-only}

## 웹서비스 런칭 개념

- 언제나 요청에 응답할 수 있으려면
    - 웹서버(컴퓨터)가 항상 켜져있어야 한다.
    - 모두가 접근할 수 있는 <span style='background-color: #f5f0ff'>공개 IP주소로 나의 웹 서비스에 접근</span>이 가능해야 한다.
- 이를 위해 AWS 같은 클라우드 서비스로 서버를 관리

## AWS 서버 구매
- 인스턴스 생성해서 원하는 OS 서버 시작
- 리눅스 Ubuntu 20.04 사용
- 인스턴스 중지: 컴퓨터 끄기
- 인스턴스 종료: 컴퓨터 반납

## AWS 실행
- Git Bash 접속
- 접속 명령어 입력
```bash
ssh -i '키경로' ubuntu@ip번호
```

## 서버 설정
```bash
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

# python3 -> python
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 10

# pip3 -> pip
sudo apt-get update
sudo apt-get install -y python3-pip
sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

# port forwarding - 80v포트를 5000포트로 전달
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 5000
```

## 파일질라 연결
- 사이트 관리 - 사이트 추가
- 프로토콜: SFTP
- 호스트: IP 번호
- 포트 번호: 22
- 로그온 유형: 키파일
- 키 파일: 저장한 키 경로

## 웹 서버 실행
- 만들어 놓은 프로젝트 파일 파일질라로 서버에 전송
    - templates 폴더
    - static 폴더
    - app.py 파일
- flask 설치
    ```bash
    pip install flask 
    pip install pymongo
    pip install dnspython
    ```
- 포트 5000번, 80번 열기
    - AWS - 보안 그룹 - launch-wizard 실행
    - 인바운드 규칙 편집 - 포트 5000/50 (Anywhere-IPv4) 규칙 저장
- app.py 실행
    ```bash
    python app.py
    ```
    - 하지만 종료하면 웹서버가 꺼짐
- 종료하더라도 웹서버 꺼지지 않게하기
    ```bash
    nohup python app.py &
    ```
- 강제 종료
    ```bash
    ps -ef | grep 'python app.py' | awk '{print $2}' | xargs kill
    ```

## 도메인 연결
- 가비아 홈페이지 - DNS 설정 - 저장
    - 호스트: @
    - 값/위치: 웹서버 IP번호 ("http://" 없이 숫자만)

## OG태크 추가
```html
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<meta property="og:title" content="내 사이트의 제목" />
<meta property="og:description" content="보고 있는 페이지의 내용 요약" />
<meta property="og:image" content="이미지URL" />
```

## 수정하기

1. 코드 수정
2. 실행 중인 웹서버 중지
3. AWS 서버에 파일 재업로드
4. 웹서버 재실행

\* OG 태그가 수정이 안되면 [카카오톡 OG태그 초기화](https://developers.kakao.com/tool/clear/og)

끝!
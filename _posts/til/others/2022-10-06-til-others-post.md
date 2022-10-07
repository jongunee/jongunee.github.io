---
layout: post
title: aws
description: >
  aws 기본
sitemap: false
hide_last_modified: true
categories:
  - til
  - others
---

# aws

* toc
{:toc .large-only}

## 인스턴스

aws에서 인스턴스란 컴퓨터 하나를 의미한다.

인스턴스를 생성하면 key가 주어지는데 이를 통해 연결 가능


**인스턴스 원격 연결**

- mac: Microsoft Remote Desktop 앱
- windows: 윈도우 원격 데스크탑 앱

접속시 비밀번호는 key를 해독해서 입력해주면 됨

**인스턴스 종료**

- 중지
  - 컴퓨터를 잠시 끄는 것으로 데이터가 남아있음
  - 사용료는 들지 않지만 데이터 보관을 위한 약간의 비용 발생
  - 다시 사용할 것이라면 중지
  - <span style='background-color: #f5f0ff'>중지 후 다시 시작하게 되면 ip가 바뀜</span>
- 종료:
  - 컴퓨터를 아얘 없애는 것으로 데이터도 모두 삭제
  - 비용 과금 완전 차단
  - 다시 사용하지 않을 것이라면 종료


끝!
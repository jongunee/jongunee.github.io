---
layout: post
title: 크롤링, meta 태그
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - webBasic
---

# 크롤링, meta 태그

* toc
{:toc .large-only}

## meta 태그

> meta 태그는 \<head> 부분에 들어가는 태그로 눈으로 보이는(body) 외에 사이트 속성을 설명하는 태그

카톡에 링크보낼 때 간략하게 뜨는 설명이 바로 meta 태그


## 크롤링

\<meta property="og:title" content="보스 베이비 2">

```py
title = soup.select_one('meta[property="og:title"]')['content']
image = soup.select_one('meta[property="og:image"]')['content']
desc = soup.select_one('meta[property="og:description"]')['content']
```

- 이 경우에는 이렇게 크롤링 할 수 있다.

끝!
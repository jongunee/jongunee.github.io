---
layout: post
title: Python 크롤링
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - web_basic
---

# Python 크롤링

* toc
{:toc .large-only}

## requests 사용

```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

import requests  # requests 라이브러리 설치 필요

r = requests.get('http://spartacodingclub.shop/sparta_api/seoulair')
rjson = r.json()

rows = rjson['RealtimeCityAir']['row']

for row in rows:
    gu_name = row['MSRSTE_NM']
    gu_mise = row['IDEX_MVL']
    if gu_mise < 55:
        print(gu_name, gu_mise)
```
`reqest.get`으로 json API를 가져올 수 있다.

## 크롤링이란?

> 웹페이지에서 원하는 데이터를 추출하는 것을 의미. 정식 명칭은 <span style='background-color: #f5f0ff'>웹 스크래핑(Web Scrapping)</span>이 맞다. 

__영화 제목 스크래핑__

크롤링 순서
1. html을 가져오기  
    requests를 사용
2. 영화 제목만 추출하기  
    beautifulsoup을 사용

__예제 코드__

```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

import requests
from bs4 import BeautifulSoup

# 타겟 URL을 읽어서 HTML를 받아오고,
headers = {'User-Agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36'} #브라우저에서 콜을 날린 것 처럼 해줌 -> 파이썬으로 접근 불가능한 점을 해결
data = requests.get('https://movie.naver.com/movie/sdb/rank/rmovie.naver?sel=pnt&date=20210829',headers=headers)

# HTML을 BeautifulSoup이라는 라이브러리를 활용해 검색하기 용이한 상태로 만듦
# soup이라는 변수에 "파싱 용이해진 html"이 담긴 상태가 됨
# 이제 코딩을 통해 필요한 부분을 추출하면 된다.
soup = BeautifulSoup(data.text, 'html.parser')

#title = soup.select_one('#old_content > table > tbody > tr:nth-child(2) > td.title > div > a')

# select를 이용해서, tr들을 불러오기
movies = soup.select('#old_content > table > tbody > tr')

# movies (tr들) 의 반복문을 돌리기
for movie in movies:
    # movie 안에 a 가 있으면,
    a_tag = movie.select_one('td.title > div > a')
    if a_tag is not None:
        # a의 text를 찍어본다.
        print (a_tag.text)
```

- BeautifulSoup을 이용해서 필요한 부분 추출
- headers: 브라우저에서 콜을 날린 것처럼 해주어 파이썬으로 접근 불가능한 점을 해결
- select와 반복문을 통해 원하는 데이터만 추출



__예제 코드__

영화 순위와 별점도 추가 출력하기

- 순위 검사: 'old_content > table > tbody > tr:nth-child(7) > td:nth-child(1) > img'  
img 구조: '\<img alt="50" height="13" src="https://ssl.pstatic.net/imgmovie/2007/img/common/bullet_r_g50.gif" width="14"/>'
- 별점 검사: 'old_content > table > tbody > tr:nth-child(2) > td.point'

```py
for movie in movies:
    # movie 안에 a 가 있으면,
    a_tag = movie.select_one('td.title > div > a')
    if a_tag is not None:
        title = a_tag.text
        rank = movie.select_one('td:nth-child(1) > img')['alt'] 
        star = movie.select_one('td.point').text
        print(rank, title, star)
```

끝!
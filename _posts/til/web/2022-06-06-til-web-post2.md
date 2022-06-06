---
layout: post
title: MongoDB
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: false
categories:
  - til
  - web
---

# MongoDB

* toc
{:toc .large-only}

## DB 종류

1. RDBMS (SQL)
> 관계형 데이터베이스를 의미하며 엑셀에 저장하는 것과 같이 정형화 되어 있는 형태로 데이터를 저장한다.

- 정형화가 되어있기 때문에 이상한 데이터가 들어오기 힘들다.
- 검색하기 수월하다.
- 수많은 데이터 중간에 열을 삽입하기 어렵다.
- 빠르게 성장하고 변하는 스타트업 같은 경우 유연하게 대처하기가 어렵다.
- ex) MySQL

2. NoSQL (Not Only SQL)
> 딕셔너리 형태로 데이터를 저장해두는 데이터베이스를 의미하며 자유로운 형태로 데이터를 저장한다.

- 정형화가 되어있지 않아 유연하게 대처가 가능하다.
- 초기 스타트업들이 많이 선택한다.
- ex) MongoDB

## MongoDB

DB는 프로그램과 같은 것이기 때문에 컴퓨터에 설치해서 사용이 가능하다. 하지만 비즈니스를 하는 경우에 컴퓨터 하나로 관리하는 것은 힘들다.
- 유저가 몰리는 경우
- DB 백업
- 모니터링  

이와 같은 경우를 위해 클라우드 기반 DB를 선택하는 것이 좋은 선택이다.

## 초기 시작

```sql
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

from pymongo import MongoClient
import certifi

ca = certifi.where()
client = MongoClient('mongodb+srv://아이디:비밀번호@cluster0.내주소.mongodb.net/내DB명?retryWrites=true&w=majority', tlsCAFile=ca)
db = client.dbsparta

doc = {
    'name' : 'bob',
    'age' : 27
}

db.users.insert_one(doc)
```

## mongoDB CRUD

### 저장
```sql
doc = {'name':'bobby','age':21}
db.users.insert_one(doc)
```

### 한 개 찾기
```sql
user = db.users.find_one({'name':'bobby'})
```

### 여러개 찾기 (_id 값은 제외하고 출력)
```sql
all_users = list(db.users.find({},{'_id':False}))
```

### 바꾸기
```sql
db.users.update_one({'name':'bobby'},{'$set':{'age':19}})
```

### 지우기
```sql
db.users.delete_one({'name':'bobby'})
```

## 실습 - 영화 데이터 db 저장

```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

for movie in movies:
    a_tag = movie.select_one('td.title > div > a')
    if a_tag is not None:
        title = a_tag.text
        rank = movie.select_one('td:nth-child(1) > img')['alt']
        star = movie.select_one('td.point').text
        print(rank, title, star)
        doc = {
            'title' : title,
            'rank' : rank,
            'star' : star
        }
        db.movies.insert_one(doc)
```

## 퀴즈

1. 영화제목 '가버나움' 평점 가져오기

```py
res = db.movies.find_one({'title':'가버나움'})
print(res['star'])
```

2. '가버나움' 평점과 같은 평점의 영화 제목들 가져오기

```py
score = db.movies.find_one({'title':'가버나움'})['star']
all_movies = list(db.movies.find({'star' : score}))
for movie in all_movies:
    print(movie['title'])
```

3. '가버나움' 평점 0으로 만들기

```py
db.movies.update_one({'title': '가버나움'}, {'$set': {'star': '0'}})
```

## 지니뮤직 크롤링 + db 저장

```py
for song in songs:
    rank = song.select_one('td.number').text[0:2].rstrip()
    title = song.select_one('td.info > a.title.ellipsis').text.strip().lstrip("19금").lstrip()
    artist = song.select_one('td.info > a.artist.ellipsis').text
    print(rank, title, artist)

    doc = {
        'rank': rank,
        'title': title,
        'artist': artist
    }
    db.songs.insert_one(doc)
```
- `text[0:2]`: 2 글자만 출력
- `lstrip()`: 왼쪽 공백 제거
- `rstrip()`: 오른쪽 공백 제거
- `strip()`: 양쪽 공백 제거


끝!
---
layout: post
title: CSS 실습 2
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: false
categories:
  - til
  - web
---

# CSS 실습 2

* toc
{:toc .large-only}

## 영화 리뷰 구현하기

### body - wrap 카드 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<div class="wrap">
    <div class="row row-cols-1 row-cols-md-4 g-4">
        <div class="col">
        <div class="card">
            <img src="https://movie-phinf.pstatic.net/20210728_221/1627440327667GyoYj_JPEG/movie_image.jpg
" class="card-img-top" alt="...">
            <div class="card-body">
            <h5 class="card-title">제목</h5>
            <p class="card-text">내용</p>
            <p>⭐⭐⭐</p>>
            <p class="mycomments">코멘트</p>>
            </div>
        </div>
    </div>
    ...
</div>
```

- 코드스니펫에서 카드 코드 가져와서 사용
- `row-cols-1 row-cols-md-4` 3에서 4로 변경해서 카드 4개로 맞춰준다
- 이미지 삽입
- 레이팅 용 별 삽입
- 참조 : [이모티콘 모음](https://kr.piliapp.com/facebook-symbols/)

### head-style 폰트 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.mycomments{
    color: gray;
}

```

- 코멘트 색상 변경

### head-style 카드 레이아웃 조정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.wrap{
    width: 1200px;
    margin: 20px auto 0px auto;
}
```

### 최종 결과

![그림1](/assets/img/web/css_card.png)


### 최종 코드

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
          integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
            crossorigin="anonymous"></script>
    <title>스파르타코딩클럽 | 부트스트랩 연습하기</title>
    <link href="https://fonts.googleapis.com/css2?family=Gowun+Dodum&display=swap" rel="stylesheet">

    <style>
        * {
            font-family: 'Gowun Dodum', sans-serif;
        }

        .mytitle{
            background-color: green;

            width: 100%;
            height: 250px;

            background-image: linear-gradient(0deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5))
, url("https://movie-phinf.pstatic.net/20210715_95/1626338192428gTnJl_JPEG/movie_image.jpg");
            background-position: center;

            color: white;

            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .mytitle > button{
            width: 200px;
            height: 50px;

            background-color: transparent;
            color: white;

            border-radius: 50px;

            border: 1px solid white;

            margin-top: 10px;
        }
        .mytitle > button:hover{
            border: 2px solid white;
        }
        .mycomments{
            color: gray;
        }
        .wrap{
            width: 1200px;
            margin: 20px auto 0px auto;
        }
    </style>

</head>
<body>
    <div class="mytitle">
        <h1>내 생애 최고의 영화들</h1>
        <button>영화 기록하기</button>
    </div>
    <div class="wrap">
        <div class="row row-cols-1 row-cols-md-4 g-4">
            <div class="col">
            <div class="card">
              <img src="https://movie-phinf.pstatic.net/20210728_221/1627440327667GyoYj_JPEG/movie_image.jpg
    " class="card-img-top" alt="...">
              <div class="card-body">
                <h5 class="card-title">제목</h5>
                <p class="card-text">내용</p>
                <p>⭐⭐⭐</p>>
                <p class="mycomments">코멘트</p>>
              </div>
            </div>
          </div>
            <div class="col">
            <div class="card">
              <img src="https://movie-phinf.pstatic.net/20210728_221/1627440327667GyoYj_JPEG/movie_image.jpg
    " class="card-img-top" alt="...">
              <div class="card-body">
                <h5 class="card-title">제목</h5>
                <p class="card-text">내용</p>
                <p>⭐⭐⭐</p>>
                <p class="mycomments">코멘트</p>>
              </div>
            </div>
          </div>
            <div class="col">
            <div class="card">
              <img src="https://movie-phinf.pstatic.net/20210728_221/1627440327667GyoYj_JPEG/movie_image.jpg
    " class="card-img-top" alt="...">
              <div class="card-body">
                <h5 class="card-title">제목</h5>
                <p class="card-text">내용</p>
                <p>⭐⭐⭐</p>>
                <p class="mycomments">코멘트</p>>
              </div>
            </div>
          </div>
            <div class="col">
            <div class="card">
              <img src="https://movie-phinf.pstatic.net/20210728_221/1627440327667GyoYj_JPEG/movie_image.jpg
    " class="card-img-top" alt="...">
              <div class="card-body">
                <h5 class="card-title">제목</h5>
                <p class="card-text">내용</p>
                <p>⭐⭐⭐</p>>
                <p class="mycomments">코멘트</p>>
              </div>
            </div>
          </div>
        </div>
    </div>>

</body>
</html>
```

끝!
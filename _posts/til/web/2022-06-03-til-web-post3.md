---
layout: post
title: CSS 실습 3
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: false
categories:
  - til
  - web
---

# CSS 실습 3

* toc
{:toc .large-only}

## 영화 리뷰 구현하기

### body - mypost 포스팅 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<div class="mypost">
    <div class="form-floating mb-3">
        <input type=url class="form-control" id="floatingInput" placeholder="name@example.com">
        <label for="floatingInput">영화URL</label>
    </div>
    <div class="input-group mb-3">
        <label class="input-group-text" for="inputGroupSelect01">별점</label>
        <select class="form-select" id="inputGroupSelect01">
            <option selected>-- 선택하기 --</option>
            <option value="1">⭐</option>
            <option value="2">⭐⭐</option>
            <option value="3">⭐⭐⭐</option>
            <option value="4">⭐⭐⭐⭐</option>
            <option value="5">⭐⭐⭐⭐⭐</option>
        </select>
    </div>
    <div class="form-floating">
            <textarea class="form-control" placeholder="Leave a comment here" id="floatingTextarea2"
                      style="height: 100px"></textarea>
        <label for="floatingTextarea2">코멘트</label>
    </div>
    <div class="button-wrap">
        <button type="button" class="btn btn-dark">기록하기</button>
        <button type="button" class="btn btn-outline-dark">닫기</button>
    </div>
</div>
```

- 코드스니펫에서 코드 가져와서 사용
- url 입력 부분
- 별점 매기기 부분
- 코멘트 남기기
- 버튼 추가

### head-style 버튼 레이아웃 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.button-wrap {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}

.button-wrap > button {
    margin: 0 5px 0 5px;
}

```

- 버튼 수평으로 정렬
- Margin 주기

### head-style 모바일 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.wrap {
    max-width: 1200px;
    width: 95%;
    margin: 20px auto 0px auto;
}

.mypost {
    max-width: 500px;
    width: 95%;

    margin: 20px auto 0px auto;

    box-shadow: 0px 0px 3px 0px gray;
    padding: 20px;

    color: gray;
}
```

- max-width로 최대 넓이 지정
- 최대 넓이 이전에는 95%로 지정

### 최종 결과

![그림1](/assets/img/web/css_final.png)


### 최종 코드

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<<!doctype html>
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

        .mytitle {
            background-color: green;

            width: 100%;
            height: 250px;

            background-image: linear-gradient(0deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url("https://movie-phinf.pstatic.net/20210715_95/1626338192428gTnJl_JPEG/movie_image.jpg");
            background-position: center;

            color: white;

            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .mytitle > button {
            width: 200px;
            height: 50px;

            background-color: transparent;
            color: white;

            border-radius: 50px;

            border: 1px solid white;

            margin-top: 10px;
        }

        .mytitle > button:hover {
            border: 2px solid white;
        }

        .mycomments {
            color: gray;
        }

        .wrap {
            max-width: 1200px;
            width: 95%;
            margin: 20px auto 0px auto;
        }

        .mypost {
            max-width: 500px;
            width: 95%;

            margin: 20px auto 0px auto;

            box-shadow: 0px 0px 3px 0px gray;
            padding: 20px;

            color: gray;
        }

        .button-wrap {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            margin-top: 20px;
        }

        .button-wrap > button {
            margin: 0 5px 0 5px;
        }
    </style>

</head>
<body>
<div class="mytitle">
    <h1>내 생애 최고의 영화들</h1>
    <button>영화 기록하기</button>
</div>
<div class="mypost">
    <div class="form-floating mb-3">
        <input type=url class="form-control" id="floatingInput" placeholder="name@example.com">
        <label for="floatingInput">영화URL</label>
    </div>
    <div class="input-group mb-3">
        <label class="input-group-text" for="inputGroupSelect01">별점</label>
        <select class="form-select" id="inputGroupSelect01">
            <option selected>-- 선택하기 --</option>
            <option value="1">⭐</option>
            <option value="2">⭐⭐</option>
            <option value="3">⭐⭐⭐</option>
            <option value="4">⭐⭐⭐⭐</option>
            <option value="5">⭐⭐⭐⭐⭐</option>
        </select>
    </div>
    <div class="form-floating">
            <textarea class="form-control" placeholder="Leave a comment here" id="floatingTextarea2"
                      style="height: 100px"></textarea>
        <label for="floatingTextarea2">코멘트</label>
    </div>
    <div class="button-wrap">
        <button type="button" class="btn btn-dark">기록하기</button>
        <button type="button" class="btn btn-outline-dark">닫기</button>
    </div>

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
                    <p>⭐⭐⭐</p>
                    <p class="mycomments">코멘트</p>
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
                    <p>⭐⭐⭐</p>
                    <p class="mycomments">코멘트</p>
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
                    <p>⭐⭐⭐</p>
                    <p class="mycomments">코멘트</p>
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
                    <p>⭐⭐⭐</p>
                    <p class="mycomments">코멘트</p>
                </div>
            </div>
        </div>
    </div>
</div>

</body>
</html>
```

끝!
---
layout: post
title: CSS 실습
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: false
categories:
  - til
  - web
---

# CSS 실습

* toc
{:toc .large-only}

## 영화 리뷰 구현하기

### body 구현
```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<body>
    <div class="mytitle">
        <h1>내 생애 최고의 영화들</h1>
        <button>영화 기록하기</button>
    </div>
</body>
```

### head - style 배경 스타일 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.mytitle{
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
```

- 배경에 이미지 삽입
- 배경 어둡게
- 글자색 흰색
- 레이아웃 컨텐츠 수직, 중앙 정렬


### head-style 버튼 스타일 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.mytitle > button{
    width: 200px;
    height: 50px;

    background-color: transparent;
    color: white;

    border-radius: 50px;

    border: 1px solid white;

    margin-top: 10px;
}
```

- 버튼 크기
- 버튼 배경 투명
- 버튼 모서리 둥그렇게
- 버튼 경계선 흰색

### head-style 버튼 클릭할 때 스타일 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.mytitle > button:hover{
    border: 2px solid white;
}
```

- 버튼 경계선 굵게 설정


### head-style 폰트 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<head>
    ...

    <title>스파르타코딩클럽 | 부트스트랩 연습하기</title>
    <link href="https://fonts.googleapis.com/css2?family=Gowun+Dodum&display=swap" rel="stylesheet">

    <style>
        * {
            font-family: 'Gowun Dodum', sans-serif;
        }
        ...
    </style>
</head>
```

- 구글 웹폰트 가져오기

### 최종 결과

![그림1](/assets/img/web/css_result.png)


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
    </style>

</head>
<body>
    <div class="mytitle">
        <h1>내 생애 최고의 영화들</h1>
        <button>영화 기록하기</button>
    </div>
</body>
</html>
```


---
layout: post
title: HTML 기초
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - web_basic
---

# HTML 기초

* toc
{:toc .large-only}

## HTML이란

> HTML은 구역과 텍스트를 나타내는 코드이다.

연습
- frontend 파일 생성
- PyCharm에서 파일 열기
- frontend - prac.html 생성

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>첫 웹페이지</title>
</head>
<body>
  내용 들어감
</body>
</html>
```

- 기본적으로 `<head>` 열기 `</head>` 닫기로 구성됨
- \<head>: 제목이나 favicon 등 설정
- \<body>: 웹페이지 내에서 내용, 속성 등 설정

## HTML 예시


```html
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 기초</title>
</head>
<body>
    <!-- 구역을 나누는 태그들 -->
    <div>나는 구역을 나누죠</div>
    <p>나는 문단이에요</p>
    <ul>
        <li> bullet point!1</li>
        <li> bullet point!2</li>
    </ul>
    <!-- 구역 내 콘텐츠 태그들 -->
    <h1>h1은 제목을 나타내는 태그입니다. 페이지마다 하나씩 꼭 써주는 게 좋아요. 그래야 구글 검색이 잘 되거든요.</h1>
    <h2>h2는 소제목입니다.</h2>
    <h3>h3~h6도 각자의 역할이 있죠. 비중은 작지만..</h3>
    <hr>
    span 태그입니다: 특정 <span style="color:red">글자</span>를 꾸밀 때 써요
    <hr>
    a 태그입니다: <a href="http://naver.com/"> 하이퍼링크 </a>
    <hr>
    img 태그입니다: <img src="https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png"/>
    <hr>
    input 태그입니다: <input type="text"/>
    <hr>
    button 태그입니다:
    <button> 버튼입니다</button>
    <hr>
    textarea 태그입니다: <textarea>나는 무엇일까요?</textarea>
</body>
</html>
```


__퀴즈 풀기__ - 로그인 페이지 만들기

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>로그인 페이지</h1>
    <p><b>ID:</b> <input type="text"/></p>
    <p><b>PW:</b> <input type="text"/></p>
    <button>로그인하기</button>
</body>
</html>
```

__결과__
![그림1](/assets/img/web/login.png)

## CSS

> CSS는 구역을 꾸며주는 역할을 한다. `<style>` 내용 `</style>`

__예제__
```html
<head>
    ...
    <style>
        .mytitle{
            color: red;
        }
    </style>
</head>

<body>
    <h1 class="mytitle">로그인 페이지</h1>
    ...
</body>
```
`<body>` 구역에 클래스명을 정해주고 `<head>` 구역에서 style을 구현해 준다. 그 결과 로그인 페이지의 글씨가 빨간색으로 출력된다.

__결과__

![그림2](/assets/img/web/style_red.png)

## 자주 사용되는 CSS 속성

1. 배경관련
    - background-color
    - background-image
    - background-size
2. 사이즈
    - width
    - height
3. 폰트
    - font-size
    - font-weight
    - font-famliy
    - color
4. 간격
    - margin
    - padding

끝!
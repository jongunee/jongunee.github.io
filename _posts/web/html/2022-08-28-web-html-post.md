---
layout: post
title: HTML 태그
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# HTML 태그

- toc
{:toc .large-only}

## HTML이란?

> 컴퓨터는 인간의 언어를 이해하지 못한다. HTML은 Markup Language로 컨셉은 브라우저에게 컨텐츠에 대한 설명을 해주는 것

## CSS란?

> Cascading Style Sheet의 약자로 컨셉은 브라우저에게 웹 사이트가 어떻게 보여야하는지에 대해 설명해주는 것

## Javascript란?

> Javascript란 웹 사이트의 두뇌 역할을 한다. 컨셉은 동적 상호작용성을 부여하여 애니매이션 등 좀 더 동적으로 동작할 수 있도록 해주는 것

## HTML 태그

- HTML을 구성하기 위해서 태그를 사용한다
- 태그 안의 것들이 Contents
- 코드를 제대로 된 위치에 넣고 tag들을 기억하면 웹 브라우저가 tag에 맞춰 변경해서 보여주는 것

**\<h>태그**

```html
<h1>Hello! this is my web browser</h1>
<h2>Hello! this is my web browser</h2>
<h3>Hello! this is my web browser</h3>
```


- 웹 브라우저는 태그 단위로 이해를 하며 h1 태그는 이미 정의 되어있기 때문에 지정된 타이틀 스타일로 바꾸어 보여줌
- h태그는 타이틀 스타일로 숫자가 커질수록 글꼴이 작아짐

**\<ul> 태그**

```html
<ul>
  <li>김치</li>
  <li>고기</li>
  <li>콜라</li>
</ul>
```

- `<ul>` 태그에 컨텐츠들은 목록으로 변경되어 보여줌
- `<li>` 태그는 list item을 의미하는 태그
- `<ul>` 태그 대신에 `<ol>` 태그로 설정하면 ordered list로 번호를 주어 순서를 정해줌

**기본 \<head>태그 및 \<body> 태그**

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <link
      rel="shortcut icon"
      href="https://assets.nflxext.com/us/ffe/siteui/common/icons/nficon2016.png" />
    <title>Home - My first website.</title>
    <meta name="description" content="This is my website" />
    <meta charset="utf-8" />
  </head>
  <body>
    <h1>Hello!</h1>
    <a href="http://google.com" target="_blank">Go to google.com</a>
    <img
      src="https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png" />
  </body>
</html>

```

- 기본적으로 브라우저에 text 타입이 아니라 html 문서라는 것을 표현
- \<html> 태그를 통해 contents가 html 코드임을 알림
- \<head>에 \<title> 태그를 통해 탭에 제목 설정
- `lang` 속성은 검색 엔진에 어떤 언어로 이루어져 있는 웹페이지인지 알려줌
- `link` 속성에 이미지 링크를 연결해 탭 아이콘 설정 가능
- \<head> 태그 안에 있는 \<meta> 태그의 내용들은 non-visible이기 떄문에 보이지 않지만 검색 엔진이 검색 할 떄 사용됨
- `charset` 속성을 text 속성을 나타내는데 다른 언어로 작성되었을 때 깨질 수 있기 때문에 항상 설정해 주는 것이 좋음
- 기본적으로 내용들은 \<body> 태그 내부에 작성

**form 태그**

```html
<body>
  <form>
    <label for="profile">Profile Photo</label>
    <input id="profile" type="file" accept="image/*,.pdf" />
    <input type="color" />
    <input required placeholder="User name" type="text" />
    <input placeholder="Password" minlength="10" type="password" />
    <input type="submit" value="Create Account" />
  </form>
</body>
```

- `file` 타입으로 파일을 불러올 수 있고 `accept` 속성으로 파일 형식 선택 
- `color` 타입은 색상 선택기를 오픈
- `label`을 지정하고 `id`를 동일하게 해주면 Profile Photo 글자를 클릭해도 파일을 불러올 수 있음
- `placeholder` 속성으로 임시 의미 표시
- `requred`를 붙이면 반드시 값을 채우도록 검증할 수 있음 
- `password` 타입은 입력되는 값을 가림
- `minlength` 속성으로 최소 길이 설정
- `submit` 버튼 타입

**Semantic 태그**

```html
<body>
  <header>
    <h1>Jongwon Park</h1>
  </header>
  <main>hello!</main>
  <footer>&copy; 2022 J.W</footer>
</body>
```

- \<header>, \<main>, \<footer> 태그들 모두 \<div> 태그와 같은 동작을 함
- \<div> 태그로 모두 작성을 해도 되지만 코드의 가독성을 위해 구분지음
- 이것들이 바로 Semantic Tags





좀 더 상세한 내용은 html web documents에서 확인 가능하다.

링크: <https://developer.mozilla.org/ko/docs/Web/HTML>

<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
